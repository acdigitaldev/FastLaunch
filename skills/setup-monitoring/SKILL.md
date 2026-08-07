---
description: |
  Sets up Sentry error monitoring for the webapp. Use when the user says "set up error monitoring," "set up Sentry," "add crash reporting," "track errors in production," or when launch-webapp reaches the monitoring step. Installs and configures Sentry for Next.js â covers client errors, server errors, API route errors, and performance tracing.
---

# Error monitoring â Sentry

## Why this matters

Without error monitoring, you find out about production bugs from angry users. Sentry catches errors in real-time, groups them by frequency, and shows you the exact stack trace and user context â turning 4-hour debugging sessions into 10-minute fixes.

---

## What to collect

- Sentry account (free tier: 5,000 errors/month â plenty for early stage)
- Project name (usually matches the product name)

---

## Step 1 â Create Sentry project (Claude in Chrome)

1. Navigate to sentry.io â Create account (or sign in)
2. New Project â Next.js
3. Project name: `[product-name]-web`
4. Alert frequency: Alert me on every new issue (change later)
5. Copy **DSN** (looks like: `https://abc123@o123.ingest.sentry.io/456`)

---

## Step 2 â Install Sentry

```bash
npx @sentry/wizard@latest -i nextjs
```

When prompted: connect to Sentry, add performance monitoring, add Session Replay, skip CI/CD.

---

## Step 3 â Manual config (if wizard wasn't used)

```bash
npm install @sentry/nextjs
```

```ts
// sentry.client.config.ts
import * as Sentry from '@sentry/nextjs'

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: process.env.NODE_ENV === 'production' ? 0.1 : 1.0,
  replaysSessionSampleRate: 0.1,
  replaysOnErrorSampleRate: 1.0,
  integrations: [Sentry.replayIntegration({ maskAllText: false })],
})
```

```ts
// sentry.server.config.ts + sentry.edge.config.ts
import * as Sentry from '@sentry/nextjs'
Sentry.init({ dsn: process.env.NEXT_PUBLIC_SENTRY_DSN, tracesSampleRate: 0.1 })
```

```ts
// next.config.ts
import { withSentryConfig } from '@sentry/nextjs'
export default withSentryConfig(nextConfig, {
  org: '[your-sentry-org]',
  project: '[product-name]-web',
  widenClientFileUpload: true,
  tunnelRoute: '/monitoring',
  hideSourceMaps: true,
})
```

---

## Step 4 â Identify users in errors

```ts
// lib/sentry/identify.ts
import * as Sentry from '@sentry/nextjs'
export function identifyUser(user: { id: string; email: string }) {
  Sentry.setUser({ id: user.id, email: user.email })
}
export function clearUser() { Sentry.setUser(null) }
```

Call `identifyUser()` after Supabase auth, `clearUser()` on logout.

---

## Step 5 â Error boundary

```tsx
// components/error-boundary.tsx
 'client'
import * as Sentry from '@sentry/nextjs'
import { useEffect } from 'react'
import { Button } from '@/components/ui/button'

export default function ErrorBoundary({ error, reset }) {
  useEffect(() => { Sentry.captureException(error) }, [error])
  return (
    <div className="flex flex-col items-center justify-center min-h-[400px] gap-4">
      <h2>Something went wrong</h2>
      <p className="text-sm text-muted-foreground">We've been notified and will fix this soon.</p>
      <Button onClick={reset}>Try again</Button>
    </div>
  )
}
```

Add `error.tsx` to `app/[locale]/(app)/` and `app/[locale]/(marketing)/`.

---

## Step 6 â Environment variables

```
NEXT_PUBLIC_SENTRY_DS=https://abc123@o123.ingest.sentry.io/456
DÔÑTRY_ORG=[your-org-slug]
SENTRY_PROJECT=[project-name]
SENTRY_AUTH_TOKEN=[from sentry.io/settings/auth-tokens]
```

Add `SENTRY_AUTH_TOKEN` to Vercel env vars (needed for source map uploads).

---

## Step 7 â Alerts (Claude in Chrome)

In Sentry -> Alerts -> Create Alert Rule:

- **New error in production**: new issue created, env.production -> email immediately
- **Error spike**: >100 events/hour -> email immediately
- **Slow pages**: P95 response > 3000ms -> email

---

## After setup

- Production errors appear in Sentry within seconds
- "Set up uptime monitoring?" -> Sentry Crons or betteruptime.com (free)
- "Set up a status page?" -> betteruptime.com has free public status pages
