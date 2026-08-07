---
description: |
  Adds GDPR compliance to the webapp and marketing site: cookie consent banner, privacy policy, terms of service, and GA4 consent mode. Use when the user says "add GDPR," "cookie consent," "cookie banner," "privacy policy," "terms of service," "we need GDPR compliance," or when launch-webapp reaches the compliance step.
---

# GDPR + cookie consent setup

## What this covers

- Cookie consent banner (accept/decline granular categories)
- GA4 consent mode integration (analytics only fires after consent)
- Privacy policy page (generated template)
- Terms of service page (generated)
- Cookie policy linked from the banner

---

## Step 1 â Cookie consent component

### Option A: Cookieyes (recommended)

1. Sign up at cookieyes.com
2. Add domain â it scans cookies automatically
3. Copy the embed script
4. Add to app/[locale]/layout.tsx:

```tsx
import Script from 'next/script'

<Script
  id="cookieyes"
  src="https://cdn-cookieyes.com/client_data/[your-id]/script.js"
  strategy="beforeInteractive"
  />
```

Cookieyes handles consent mode, banner styling, and cookie blocking automatically.

### Option B: Custom consent component

```tsx
// components/cookie-consent/CookieBanner.tsx
'use client'
import { useState, useEffect } from 'react'
import { Button } from '@/components/ui/button'
import { updateConsent } from '@/lib/analytics/consent'

export function CookieBanner() {
  const [visible, setVisible] = useState(false)

  useEffect(() => {
    const consent = localStorage.getItem('cookie-consent')
    if (!consent) setVisible(true)
  }, [])

  const accept = () => {
    localStorage.setItem('cookie-consent', 'accepted')
    updateConsent(true)
    setVisible(false)
  }

  const decline = () => {
    localStorage.setItem('cookie-consent', 'declined')
    updateConsent(false)
    setVisible(false)
  }

  if (!visible) return null

  return (
    <div className="fixed bottom-0 left-0 right-0 z-50 p4 bg-background border-t shadow-lg md:flex md:items-center md:justify-between">
      <p className="text-sm text-muted-foreground mb-3 md:mb-0 md:mr-8">
        We use cookies to analyze traffic and improve your experience.{
        ' '}~<a href="/privacy" className="underline">Privacy policy</a>
      </p>
      <div className="flex gap-2 shrink-0">
        <Button variant="outline" size="sm" onClick={decline}>Decline</Button>
        <Button size="sm" onClick={accept}>Accept cookies</Button>
      </div>
    </div>
  )
}
```

---

## Step 2 â GA4 consent mode

```ts
// lib/analytics/consent.ts
export function initConsentMode() {
  window.gtag?.('consent', 'default', {
    analytics_storage: 'denied',
    ad_storage: 'denied',
    security_storage: 'granted',
  })
}

export function updateConsent(granted: boolean) {
  window.gtag?.('consent', 'update', {
    analytics_storage: granted ? 'granted' : 'denied',
    ad_storage: 'denied',
  })
}
```

---

## Step 3 â Privacy policy page

Generate /app/[locale]/(marketing)/privacy/page.tsx with:

1. Who we are
2. What data we collect (email, name, usage data via GA4, payment via Stripe)
3. How we use your data (to provide service, send transactional emails)
4. Data storage and security (Supabase EU/US, Stripe PCI DSS, no passwords stored)
5. Your rights (GDPR): access, delete, portability, opt-out of analytics
6. Cookies (link to cookie policy)
7. Third-parties: Supabase, Stripe, Resend, Google Analytics
8. Contact: privacy@[domain]

---

## Step 4 â Terms of service page

Generate /app/[locale]/(marketing)/terms/page.tsx with:

1. Acceptance of terms
2. Description of service
3. User accounts
4. Acceptable use
5. Payment terms (7-day refund recommended)
6. Intellectual property (your data is yours)
7. Limitation of liability
8. Termination
9. Governing law
10. Contact

**Note**: These are a starting point. For production SaaS with significant revenue, have a lawyer review them.

---

## Step 5 â Add compliance links to footer

```tsx
<div className="flex gap-4 text-sm text-muted-foreground">
  <a href="/privacy">Privacy Policy</a>
  <a href="/terms">Terms of Service</a>
  <a href="/cookies">Cookie Policy</a>
</div>
```

---

## Multilingual compliance pages

Generate translated versions for each configured locale. Privacy policy and terms must be in the user's language in the EU for full compliance.

---

## After setup

- Cookie consent is live and GA4 respects it
- Privacy policy and terms are indexed
