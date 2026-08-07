---
description: |
  Sets up Stripe billing with subscription tiers, customer portal, and webhook integration. Use when the user says "set up Stripe," "add billing," "set up payments," "set up subscriptions," "add Stripe," or when launch-webapp reaches the payments step. Covers: Stripe products + prices, checkout session, customer portal, webhook handler, and billing settings page in the app.
---

# Stripe billing setup

## What to collect

- Pricing tiers (name, monthly price, annual price, features per tier)
- Free trial? If yes, how many days?
- Metered billing? (seat-based, usage-based, or flat subscription)
- Currency (default: USD)
- Do they have a Stripe account? If not, guide creation

---

## Step 1 - Create Stripe products + prices (Claude in Chrome)

Navigate to: dashboard.stripe.com/products

For each pricing tier:
1. Products -> Add product
2. Name: [Tier name] (e.g., "Pro", "Team")
3. Description: [features list]
4. Pricing model: Recurring
5. Price: monthly amount
6. Add another price: annual amount (monthly x 10 for 2 months free)
7. Save -> copy Price IDs

Collect all Price IDs:
```
STRIPE_PRO_MONTHLY_PRICE_ID=price_xxx
STRIPE_PRO_ANNUAL_PRICE_ID=price_xxx
STRIPE_TEAM_MONTHLY_PRICE_ID=price_xxx
STRIPE_TEAM_ANNUAL_PRICE_ID=price_xxx
```

---

## Step 2 - Install Stripe in Next.js

```bash
npm install stripe @stripe/stripe-js
```

```ts
// lib/stripe/client.ts
import Stripe from 'stripe'

export const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!, {
  apiVersion: '2024-06-20',
  typescript: true,
})
```

```ts
// lib/stripe/plans.ts
export const PLANS = {
  free: {
    name: 'Free',
    monthlyPriceId: null,
    annualPriceId: null,
    features: ['Up to 3 projects', 'Basic features', 'Community support'],
  },
  pro: {
    name: 'Pro',
    monthlyPriceId: process.env.STRIPE_PRO_MONTHLY_PRICE_ID!,
    annualPriceId: process.env.STRIPE_PRO_ANNUAL_PRICE_ID!,
    price: { monthly: 29, annual: 290 },
    features: ['Unlimited projects', 'Advanced features', 'Priority support'],
  },
  team: {
    name: 'Team',
    monthlyPriceId: process.env.STRIPE_TEAM_MONTHLY_PRICE_ID!,
    annualPriceId: process.env.STRIPE_TEAM_ANNUAL_PRICE_ID!,
    price: { monthly: 79, annual: 790 },
    features: ['Everything in Pro', 'Up to 10 members', 'Admin panel', 'SSO (coming soon)'],
  },
} as const
```

---

## Step 3 - Checkout session API route

```ts
// app/api/stripe/checkout/route.ts
import { stripe } from '@/lib/stripe/client'
import { createServerClient } from '@/lib/supabase/server'
import { NextResponse } from 'next/server'

export async function POST(req: Request) {
  const supabase = createServerClient()
  const { data: { user } } = await supabase.auth.getUser()
  if (!user) return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })

  const { priceId, workspaceId } = await req.json()

  const session = await stripe.checkout.sessions.create({
    customer_email: user.email,
    mode: 'subscription',
    payment_method_types: ['card'],
    line_items: [{ price: priceId, quantity: 1 }],
    success_url: `${process.env.NEXT_PUBLIC_APP_URL}/settings/billing?success=true`,
    cancel_url: `${process.env.NEXT_PUBLIC_APP_URL}/settings/billing`,
    metadata: { userId: user.id, workspaceId },
    subscription_data: {
      trial_period_days: 14, // remove if no trial
      metadata: { userId: user.id, workspaceId },
    },
  })

  return NextResponse.json({ url: session.url })
}
```

---

## Step 4 - Customer portal

```ts
// app/api/stripe/portal/route.ts
export async function POST(req: Request) {
  const supabase = createServerClient()
  const { data: { user } } = await supabase.auth.getUser()

  const { data: workspace } = await supabase
    .from('workspaces')
    .select('stripe_customer_id')
    .eq('owner_id', user!.id)
    .single()

  const session = await stripe.billingPortal.sessions.create({
    customer: workspace!.stripe_customer_id,
    return_url: `${process.env.NEXT_PUBLIC_APP_URL}/settings/billing`,
  })

  return NextResponse.json({ url: session.url })
}
```

---

## Step 5 - Webhook handler

```ts
// app/api/webhooks/stripe/route.ts
import { stripe } from '@/lib/stripe/client'
import { createAdminClient } from '@/lib/supabase/admin'

export async function POST(req: Request) {
  const body = await req.text()
  const sig = req.headers.get('stripe-signature')!

  let event: Stripe.Event
  try {
    event = stripe.webhooks.constructEvent(body, sig, process.env.STRIPE_WEBHOOK_SECRET!)
  } catch {
    return new Response('Webhook signature verification failed', { status: 400 })
  }

  const supabase = createAdminClient()

  switch (event.type) {
    case 'checkout.session.completed': {
      const session = event.data.object as Stripe.CheckoutSession
      const { workspaceId } = session.metadata!
      await supabase.from('workspaces').update({
        plan: 'pro',
        stripe_customer_id: session.customer as string,
        stripe_subscription_id: session.subscription as string,
      }).eq('id', workspaceId)
      break
    }
    case 'customer.subscription.deleted': {
      const sub = event.data.object as Stripe.Subscription
      await supabase.from('workspaces').update({ plan: 'free' })
        .eq('stripe_subscription_id', sub.id)
      break
    }
    case 'invoice.payment_failed': {
      // Send dunning email via Resend
      break
    }
  }

  return new Response('OK')
}
```

### Register webhook in Stripe (Claude in Chrome)

1. dashboard.stripe.com -> Developers -> Webhooks -> Add endpoint
2. URL: `https://[domain]/api/webhooks/stripe`
3. Events to listen for:
   - `checkout.session.completed`
   - `customer.subscription.updated`
   - `customer.subscription.deleted`
   - `invoice.payment_failed`
   - `invoice.payment_succeeded`
4. Copy Webhook Signing Secret -> STIRPE_WEBHOOK_SECRET

---

## Step 6 - Supabase schema additions

```sql
alter table workspaces add column stripe_customer_id text;
alter table workspaces add column stripe_subscription_id text;
alter table workspaces add column plan text not null default 'free';
alter table workspaces add column plan_expires_at timestamptz;
```

---

## Step 7 - Billing settings page

Generate `/app/[locale]/(app)/settings/billing/page.tsx`:
- Show current plan + next billing date
- Upgrade button -> calls /api/stripe/checkout
- Manage billing button -> calls /api/stripe/portal
- Show feature comparison if on free plan

---

## .env additions

```
STRIPE_SECRET_KEY=sk_live_xxx (use sk_test_xxx during dev)
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_live_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx
STRIPE_PRO_MONTHLY_PRICE_ID=price_xxx
STRIPE_PRO_ANNUAL_PRICE_ID=price_xxx
STRIPE_TEAM_MONTHLY_PRICE_ID=price_xxx
STRIPE_TEAM_ANNUAL_PRICE_ID=price_xxx
```

---

## After setup

- "Connect billing to your CRM?" -> track `subscription_created` event in CRM
- "Set up dunning emails?" -> add invoice.payment_failed handler -> Resend email
- "Add upgrade nudges in the app?" -> see paywall-upgrade-cro skill
