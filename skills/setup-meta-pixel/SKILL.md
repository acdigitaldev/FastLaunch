---
description: |
  Sets up Meta Business Manager, Meta Pixel, and ad account for the webapp. Use when the user says "set up Meta Pixel," "set up Meta Business Manager," "set up Facebook Pixel," "configure Meta ads," "set up paid ads on Meta," or when launch-webapp reaches the Meta Pixel step. Covers: Business Manager creation, Pixel installation, standard event tracking, and ad account configuration.
---

# Meta Business Manager + Pixel setup

## Why this matters

Meta ads are one of the highest-ROI paid channels for B2C and prosumer SaaS. But the setup is a bureaucratic maze -- Business Manager, Pixel, Events Manager, ad account -- all need to be connected before you can run a single ad. This skill handles all of it in one go, so it's ready when you need it -- even if you're not running ads today.

---

## What to collect

- Facebook account (personal -- used to create Business Manager)
- Business name and domain
- New business or existing? (existing may already have Business Manager)

---

## Step 1 -- Create Meta Business Manager (Claude in Chrome)

1. Navigate to business.facebook.com
2. Create Account -> enter business name, your name, and hello@[domain]
3. Verify email
4. Business Manager is created -- you now have a central hub for all Meta business assets

---

## Step 2 -- Create Meta Pixel (Claude in Chrome)

1. In Business Manager -> Events Manager (left sidebar)
2. Connect Data Sources -> Web -> Meta Pixel
3. Pixel name: [Product Name] Pixel
4. Enter website URL: https://[domain]
5. Choose "Install code manually"
Copy the Pixel ID (looks like: 1234567890123456)

---

## Step 3 -- Install Pixel in Next.js

```tsx
// app/[locale]/layout.tsx
import Script from 'next/script'

// Add inside <body>:
<Script id="meta-pixel" strategy="afterInteractive">
  {`
    !function(f,b,e,v,n,t,s)
    {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
    n.callMethod.apply(n,arguments):n.queue.push(arguments)};
    if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
    n.queue=[];t=b.createElement(e);t.async=!0;
    t.src=v;s=b.getElementsByTagName(e)[0];
    s.parentNode.insertBefore(t,s)}(window, document,'script',
    'https://connect.facebook.net/en_US/fbevents.js');
    fbq('init', '${process.env.NEXT_PUBLIC_META_PIXEL_ID}');
    fbq('track', 'PageView');
  `}
</Script>
```

```
NEXT_PUBLIC_META_PIXEL_ID=1234567890123456
```

---

## Step 4 -- Standard events to track

```ts
// lib/analytics/meta-pixel.ts
export function trackEvent(event: string, params?: Record<string, unknown>) {
  if (typeof window !== 'undefined' && (window as any).fbq) {
    (window as any).fbq('track', event, params)
  }
}
```

| Trigger | Event to fire |
|---------|---------------|
| User signs up | `trackEvent('CompleteRegistration')` |
| User starts trial | `trackEvent('StartTrial', { value: 0, currency: 'USD' })` |
| User upgrades to paid | `trackEvent('Purchase', { value: [price], currency: 'USD' })` |
| User views pricing | `trackEvent('ViewContent', { content_name: 'Pricing' })` |
| User clicks main CTA | `trackEvent('Lead')` |

---

## Step 5 -- GDPR: only fire Pixel after consent

```ts
export function initMetaPixel() {
  if (typeof window !== 'undefined' && process.env.NEXT_PUBLIC_META_PIXEL_ID) {
    (window as any).fbq?.('init', process.env.NEXT_PUBLIC_META_PIXEL_ID)
    (window as any).fbq?.('track', 'PageView')
  }
}
```

Call `initMetaPixel()` from the consent `accept()` function in CookieBanner.

---

## Step 6 -- Verify Pixel is firing

1. Install Meta Pixel Helper Chrome extension
2. Visit your site -- extension shows green (Pixel active)
3. Events Manager -> Test Events -> browse site -> see events in real time

---

## Step 7 -- Create ad account (Claude in Chrome)

Even if not running ads today, create the ad account so it's aged and ready.

1. Business Manager -> Accounts -> Ad Accounts -> Add -> Create a new ad account
2. Ad account name: [Product Name]
3. Time zone: local timezone; Currency: USD
4. DO NOT add payment method yet -- only add it when ready to run ads

**Why now?** New ad accounts have spending limits and lower trust. A 30-60 day old account with zero spend still builds trust with Meta.

---

## Step 8 -- Connect domain to Business Manager

Required for iOS 14+ attribution:

1. Business Manager -> Brand Safety -> Domains -> Add domain
2. Verify via DNSTXT record in Namecheap:
   - Type: TXT, Host: @, Value: [verification code from Meta]
3. Back in Meta -> Verify domain

---

## After setup

- Pixel is live and tracking PageView on every page
- Sign-up, upgrade, and CTA events are tracked
- Ad account is created and ageing
- Domain is verified -- ready for iOS 14+ campaigns
- "Ready to run Meta ads?" -> Claude can help write ad copy and set up first campaign
- "Set up retargeting audiences?" -> Audiences in Ads Manager, create WebYttCustomAudience from Pixel data
