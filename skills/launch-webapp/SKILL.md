---
description: |
  Master orchestrator for the full solopreneur webapp launch. Use when the user says "launch my app," "run the full launch checklist," "start the launch sequence," "set up everything for my SaaS," or anything that implies starting from scratch with a new webapp. Collects product details, shows the full checklist, and invokes each sub-skill in order.
---

# Full launch checklist

## What to collect first

1. **Product name** -- what's it called?
2. **One-liner** -- what does it do in one sentence?
3. **Target audience** -- who is it for (be specific)?
4. **Tech stack** -- using Next.js + Supabase (default) or something else?
5. **Domain** -- do you have one already, or should we find one?
6. **Languages** -- English only, or multilingual? Which languages?
- **Pricing** -- free only, or paid tiers?
8. **Current state** -- idea only / built but not deployed / deployed but no marketing?

---

## Full checklist (20 steps)

```
FASTLAUNCH CHECKLIST
ââââââââââââââââââââââââââââââââââââââââ

FOUNDATION
[ ] 1.  Domain & DNS          -> setup-domain
[ ] 2.  Supabase + GitHub    -> setup-infra
[ ] 3.  Web app scaffold     -> create-webapp
[ ] 4.  Marketing site       -> create-marketing-site
[ ] 5.  Vercel deployment    -> setup-deployment

MONETIZATION
[ ] 6.  Stripe billing       -> setup-stripe

COMMUNICATION
[ ] 7.  Email auth (SPF/DKIM) -> setup-email-auth
[ ] 8.  PLG onboarding emails -> setup-onboarding-emails

GROWTH & DISCOVERY
[] 9.   Analytics (GA4/GSC/GTM)  -> setup-analytics
[ ] 10. Meta Pixel & Business   -> setup-meta-pixel
[ ] 11. GDPR / cookie consent   -> setup-gdpr
[] 12.  SEO (Rankahead)        -> setup-rankahead-seo
[] 13.  CRM                     -> setup-crm

SOCIAL PRESENCE
[ ] 14. Social accounts         -> setup-social-accounts
[ ] 15. WhatsApp bot            -> setup-whatsapp
[ ] 16. YouTube channel         -> setup-youtube

CONTENT
[ ] 17. 12 launch posts         -> first-posts
[ ] 18. 3-month content plan    -> content-plan

RELIABILITY
[ ] 19. Error monitoring (Sentry) -> setup-monitoring

ââââââââââââââââââââââââââââââââââââââââ
Estimated total: 3-6 hours (mostly waiting for DNS)
Token estimate: 80,000-140,800 tokens for full run
```

---

## Execution rules

1. **Always confirm before irreversible steps** -- anything that creates cloud resources, sends an email, or makes a purchase requires explicit "yes" from the user
2. **Run in dependency order** -- domain before deployment, email auth before email sequences, Supabase before webapp
3. **Skip steps the user already has** -- if they have Stripe, skip Step 6 setup
4. **Pause at each step** -- show what will happen, wait for approval, then run it
5. **Save state** -- after each completed step, summarize what was set up

---

## Minimal viable launch (fastest path)

If the user wants to ship in under 2 hours, run only:
1. setup-infra
2. create-webapp
3. setup-deployment
4. setup-stripe
5. setup-email-auth
6. setup-onboarding-emails
7. first-posts

---

## Step routing

| Step | Skill to invoke |
|------|-----------------|
| Domain | `setup-domain` |
| Infra | `setup-infra` |
| Webapp | `create-webapp` |
| Marketing site | `create-marketing-site` |
| Deployment | `setup-deployment` |
| Stripe | `setup-stripe` |
| Email auth | `setup-email-auth` |
| Onboarding emails | `setup-onboarding-emails` |
| Analytics | `setup-analytics` |
| Meta Pixel | `setup-meta-pixel` |
| GDPR | `setup-gdpr` |
| SEO | `setup-rankahead-seo` |
| CRM | `setup-crm` |
| Social accounts | `setup-social-accounts` |
| WhatsApp | `setup-whatsapp` |
| YouTube | `setup-youtube` |
| Launch posts | `first-posts` |
| Content plan | `content-plan` |
| Monitoring | `setup-monitoring` |

---

## After completing all steps

Generate a launch summary:
- All services set up (tick
- All env vars to add to Vercel (list them)
- Connector credentials needed
- First week action items
- What Rankahead is now handling automatically
---
description: |
  Master launch orchestrator for solopreneurs. Use when the user says "launch my app," "I just built a webapp and need to launch it," "run the full launch checklist," "set up everything for my SaaS," or "I am ready to go live." Runs the complete FastLaunch sequence: infra, onboarding emails, SEO, social posts, WhatsApp, content plan. Pauses for user approval before any irreversible action.
---

# FastLaunch — full launch sequence

Collect the following from the user before running any step. Ask all at once:

- Product name
- One-line description (what it does, for whom)
- Target audience (be specific)
- Tech stack
- Domain (if they have one)
- Current state (live, staging, or dev)

Once collected, show this checklist and confirm:

## FastLaunch checklist

- [ ] 1. Infrastructure — Supabase + GitHub
- [ ] 2. Onboarding emails — Resend PLG sequence
- [ ] 3. SEO and blog — Rankahead setup + first post
- [ ] 4. Social launch posts — Instagram, Twitter/X, LinkedIn, Threads
- [ ] 5. WhatsApp bot — WhatsChat setup guide + copy
- [ ] 6. 3-month content plan

Run each step in order. After each completes, show a checkmark and ask "Ready for the next step?" before continuing.

Invoke the corresponding skill for each step:
1. setup-infra
2. setup-onboarding-emails
3. setup-rankahead-seo
4. first-posts
5. setup-whatsapp
6. content-plan

At the end, display a summary of everything created and all credentials/links to save.

## Token estimate
A full run uses approximately 40,000-70,000 tokens (~$0.10-$0.20 at Sonnet pricing).
