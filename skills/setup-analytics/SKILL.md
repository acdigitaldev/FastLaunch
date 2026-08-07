---
description: |
  Sets up the full analytics and search stack: Google Analytics 4 property, Google Search Console, robots.txt, Google Tag Manager, and core conversion events. Use when the user says "set up analytics," "set up GA4," "connect Google Analytics," "set up Search Console," "set up GTM," "set up Google Tag Manager," "I need analytics," or when launch-webapp reaches the analytics step. Uses Claude in Chrome to complete GSC and GTM setup in the browser.
---

# Analytics + search setup

## What to collect

- Domain (e.g., myproduct.com)
- Google account to use for GA4 / GSC / GTM (must own the account)
- Are they using Supabase auth? â enables user-level tracking
- Key conversion events to track (signup, upgrade, invite sent, etc.)

---

## Step 1 â Google Analytics 4

### Create GA4 property (Claude in Chrome)

1. Navigate to analytics.google.com
2. Admin â Create â Property
3. Property name: `[Product Name]`
4. Reporting timezone: user's timezone
5. Currency: user's currency
6. Business details: fill in industry + size
7. Create Data Stream â Web â enter domain â copy **Measurement ID** (`G-XXXXXXXXXX`)

### Add GA4 to Next.js

```bash
npm install @next/third-parties
```

```tsx
// app/[locale]/layout.tsx
import { GoogleAnalytics } from '@next/third-parties/google'

export default function RootLayout({ children }) {
  return (
    <html>
      <body>{children}</body>
      <GoogleAnalytics gaId={process.env.NEXT_PUBLIC_GA_MEASUREMENT_ID!} />
    </html>
  )
}
```

```
NEXT_PUBLIC_GA_MEASUREMENT_ID=G-XXXXXXXXXX
```

### Consent mode (required for EU / GDPR)

```ts
// lib/analytics/consent.ts
export function updateConsent(granted: boolean) {
  if (typeof window === 'undefined') return
  window.gtag('consent', 'update', {
    analytics_storage: granted ? 'granted' : 'denied',
    ad_storage: 'denied', // always denied unless running ads
  })
}
```

Call `updateConsent(true)` when user accepts cookies. See setup-gdpr for the full cookie banner.

---

## Step 2 â Google Search Console

### Verify domain (Claude in Chrome)

1. Navigate to search.google.com/search-console
2. Add property â Domain verification
3. Copy the TXT record provided
4. Add to DNS: in Namecheap â Advanced DNS â Add TXT record â `@` â paste value
5. Return to GSC â Verify

### Alternatively â HTML tag verification (easier for immediate setup)

1. Add property â URL prefix â enter `https://[domain]`
2. Choose HTML tag method â copy the meta tag
3. Add to Next.js:

```tsx
// app/[locale]/layout.tsx
export const metadata = {
  verification: {
    google: '[verification-code]',
  },
}
```

4. Deploy â verify in GSC

---

## Step 3 â robots.txt

Generate `/public/robots.txt`:

```
User-agent: *
Allow: /

# Block internal app routes from indexing
Disallow: /api/
Disallow: /dashboard/
Disallow: /settings/
Disallow: /admin/
Disallow: /auth/

Sitemap: https://[domain]/sitemap.xml
```

Generate `/app/sitemap.ts`:

```ts
import { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
  const baseUrl = process.env.NEXT_PUBLIC_APP_URL!
  const locales = ['en', 'es', 'pt'] // from product config

  const marketingPages = ['', '/pricing', '/about', '/blog', '/tools']

  return locales.flatMap(locale =>
    marketingPages.map(page => ({
      url: `${baseUrl}/${locale}${page}`,
      lastModified: new Date(),
      changeFrequency: page === '/blog' ? 'daily' : 'weekly',
      priority: page === '' ? 1 : 0.8,
    }))
  )
}
```

### Submit sitemap to GSC (Claude in Chrome)

1. GSC â Sitemaps â Add sitemap URL: `https://[domain]/sitemap.xml`
2. Submit
3. Monitor indexing status in GSC â Coverage

---

## Step 4 â Google Tag Manager

### Create GTM container (Claude in Chrome)

1. Navigate to tagmanager.google.com
2. Create account â Container name: `[domain]` â Web
3. Copy **Container ID** (`GTM-XXXXXXX`)

### Add GTM to Next.js

```tsx
// app/[locale]/layout.tsx
import { GoogleTagManager } from '@next/third-parties/google'

// Add inside <html>:
<GoogleTagManager gtmId={process.env.NEXT_PUBLIC_GTM_ID!} />
```

```
NEXT_PUBLIC_GTM_ID=GTM-XXXXXXX
```

### Core tags to configure in GTM (Claude in Chrome)

In GTM â Tags â New:

**1. GA4 Configuration Tag**
- Tag type: Google Analytics: GA4 Configuration
- Measurement ID: `{{GA4 Measurement ID}}` (create constant variable)
- Trigger: All Pages

**2. GA4 Event â Signup**
- Tag type: GA4 Event
- Event name: `sign_up`
- Parameters: `method: {{signup_method}}`
- Trigger: Custom event `user_signup`

**3. GA4 Event â Trial Started**
- Event name: `begin_checkout`
- Parameters: `value: 0, currency: USD, items: [{item_name: "Pro Trial"}]`
- Trigger: Custom event `trial_started`

**4. GA4 Event â First Payment**
- Event name: `purchase`
- Parameters: `transaction_id: {{order_id}}, value: {{mrr}}, currency: USD`
- Trigger: Custom event `subscription_created`

---

## Step 5 â Fire events from your app

```ts
// lib/analytics/events.ts
export function trackEvent(name: string, params?: Record<string, unknown>) {
  if (typeof window === 'undefined') return
  window.gtag?.('event', name, params)
  // Also push to GTM dataLayer:
  window.dataLayer?.push({ event: name, ...params })
}

// Usage examples:
trackEvent('sign_up', { method: 'email' })
trackEvent('trial_started', { plan: 'pro' })
trackEvent('subscription_created', { mrr: 29, plan: 'pro' })
trackEvent('feature_used', { feature: 'export', workspace_id: id })
```

Call these from your Supabase auth hooks and Stripe webhook handlers.

---

## Step 6 â Core reports to set up in GA4 (Claude in Chrome)

1. **Conversions**: Mark `sign_up` and `purchase` as conversion events in GA4 â Admin â Conversions
2. **Funnels**: Explore â Funnel exploration â Homepage â Signup page â Dashboard (activation)
3. **Retention**: Explore â User lifetime â see if users return after day 1, 7, 30
4. **Audience**: Create audience "Trial Users" = `trial_started` event â use for remarketing

---

## Summary of what gets set up

| Tool | What's configured |
|------|-------------------|
| GA4 | Property, data stream, consent mode, conversion events |
| GSC | Domain verified, sitemap submitted, robots.txt live |
| GTM | Container, GA4 tag, 4 core event tags |
| sitemap.xml | Auto-generated, multilingual, submitted to GSC |
| robots.txt | App routes blocked, marketing routes open |

---

## After setup

- "Set up cookie consent?" â invoke setup-gdpr
- "Connect Rankahead for content insights?" â invoke setup-rankahead-seo
- "Set up social pixels?" â Facebook Pixel, LinkedIn Insight Tag via GTM
