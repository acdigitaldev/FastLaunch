---
description: |
  Deploys the webapp to Vercel and connects it to the GitHub repo for auto-deploy on push. Use when the user says "deploy my app," "set up Vercel," "connect GitHub to Vercel," "set up CI/CD," "deploy to production," or when launch-webapp reaches the deployment step. Covers: Vercel project creation, GitHub integration, environment variables, preview deployments, and production domain linking.
---

# Deployment setup - Vercel + GitHub

## What to collect

- GitHub repo URL (must exist - created in setup-infra)
- Vercel account (if none, guide creation)
- Environment variables (collected during webapp setup)
- Domain (from setup-domain - needed to link after deploy)

---

## Step 1 - Connect GitHub repo to Vercel (Claude in Chrome)

1. Navigate to vercel.com/new
2. "Import Git Repository" -> connect GitHub (first time: authorize Vercel to access repos)
3. Select the `[product-name]` repo
4. Framework preset: Next.js (auto-detected)
5. Root directory: `.` (default, or `apps/web` if monorepo)
6. Build settings: leave defaults (`next build`)

---

## Step 2 - Add environment variables

In Vercel project settings -> Environment Variables, add all of these:

**Required for production** (mark as Production + Preview):
```
NEXT_PUBLIC_SUPABASE_URL
NEXT_PUBLIC_SUPABASE_ANON_KEY
SUPABASE_SERVICE_ROLE_KEY
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY
STRIPE_SECRET_KEY
STRIPE_WEBHOOK_SECRET
NEXT_PUBLIC_APP_URL          -> set to https://[domain]
NEXT_PUBLIC_GA_MEASUREMENT_ID
NEXT_PUBLIC_GTM_ID
RESEND_API_KEY
```

**Preview-only** (for branch deploys):
```
NEXT_PUBLIC_APP_URL          -> set to https://[project].vercel.app for preview
STRIPE_SECRET_KEY            -> use Stripe test key for preview
```

---

## Step 3 - First deployment

After adding env vars:
1. Vercel -> Deployments -> trigger a new deployment (or it auto-deploys on the import)
2. Watch build logs - common errors:
   - Missing env var -> add it, redeploy
   - TypeScript error -> fix in code, push to GitHub
   - Module not found -> `npm install [package]`, push

---

## Step 4 - Connect custom domain

(Only after setup-domain is complete and DNS is configured)

1. Vercel -> Project -> Settings -> Domains
2. Add domain: `[domain.com]`
3. Add domain: `www.[domain.com]` -> set redirect to apex
4. Vercel automatically provisions SSL (Let's Encrypt) - takes 1-5 minutes
5. Verify: visit `https://[domain.com]` -> should load the app

---

## Step 5 - Configure auto-deploy branches

Vercel deploys automatically on every push. Set up branch strategy:

| Branch | Environment | URL |
|--------|-------------|-----|
| `main` | Production | https://[domain] |
| `staging` | Preview | https://staging-[project].vercel.app |
| `feature/*` | Preview | https://[branch]-[project].vercel.app |

In Vercel -> Settings -> Git:
- Production branch: `main`
- Preview branches: all others (default)

---

## Step 6 - GitHub CI workflow

The repo already has `.github/workflows/ci.yml` from setup-infra. Extend it for deployment checks:

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main, staging]
  pull_request:

jobs:
  lint-and-type-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npm run typecheck

  # Vercel deploys automatically - no deploy step needed in CI
    # GitHub Actions just validates the code before Vercel picks it up
```

Add these scripts to `package.json`:
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "typecheck": "tsc --noEmit"
  }
}
```

---

## Step 7 - Vercel speed insights + analytics (optional)

```bash
npm install @vercel/analytics @vercel/speed-insights
```

```tsx
// app/[locale]/layout.tsx
import { Analytics } from '@vercel/analytics/react'
import { SpeedInsights } from '@vercel/speed-insights/next'

// Add inside <body>:
<Analytics />
<SpeedInsights />
```

---

## Production checklist

- [ ] `NEXT_PUBLIC_APP_URL` points to production domain (not vercel.app)
- [ ] Stripe is using live keys (not test keys)
- [ ] Supabase is using the production project (not local)
- [ ] All redirect URLs in Supabase auth settings include the production domain
- [ ] Stripe webhook endpoint points to production URL
- [ ] `robots.txt` is live at `https://[domain]/robots.txt`
- [ ] `sitemap.xml` is live at `https://[domain]/sitemap.xml`

---

## After setup

- "Connect your domain?" -> invoke setup-domain if not done
- "Set up error monitoring?" -> invoke setup-monitoring
- "Set up analytics?" -> invoke setup-analytics
