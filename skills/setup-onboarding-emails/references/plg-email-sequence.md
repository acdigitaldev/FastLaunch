# PLG email sequence — reference templates

Customize [PRODUCT], [AHA_ACTION], [AUDIENCE], [DOMAIN] per product.

---

## Email 1 — Welcome (send immediately on signup)

Subject: You're in. Here's your first move.

Hey [first name],
Welcome to [PRODUCT]. You're here because [pain point]. We're going to fix that.
One thing to do right now: [AHA_ACTION]. Takes 2 minutes.
[CTA: Do this now]
Reply if stuck — I read every response.
— [Founder]

---

## Email 2 — Activation nudge (Day 1, only if not activated)

Subject: Did you get a chance to try [AHA_ACTION]?

Hey [first name],
You signed up but haven't [AHA_ACTION] yet. Here's exactly how:
1. [Step 1]  2. [Step 2]  3. [Step 3]
[CTA: Try it now]
Most people who do this come back every day.
— [Founder]

---

## Email 3 — Inspiration (Day 3)

Subject: How [AUDIENCE] use [PRODUCT] to [outcome]

Hey [first name],
Quick story. [2-3 sentence customer story with specific numbers.]
They [outcome] in [timeframe]. [PRODUCT] was the tool.
[CTA: Set up yours]
— [Founder]

---

## Email 4 — Feature discovery (Day 7)

Subject: Most people miss this in [PRODUCT]

Hey [first name],
There's a feature most people never find: [feature name]. [1 sentence what it does.]
[2-3 sentences on why it matters]
[CTA: Try it — already in your account, no upgrade needed]
— [Founder]

---

## Email 5 — Upgrade nudge (Day 14, free users only)

Subject: You've been on the free plan for 2 weeks

Hey [first name],
Free plan: [limits]. Pro unlocks: [3 specific concrete benefits].
[CTA: Upgrade — $X/month]
P.S. Price locks in for life if you upgrade now.
— [Founder]

---

## Email 6 — Win-back (Day 30, inactive)

Subject: Still there?

Hey [first name],
Haven't seen you in a while. Something didn't work? I probably fixed it.
[CTA: Come back and see]
No guilt if you want to stay free forever. Just reply either way.
— [Founder]

---

## Behavioral trigger guide

| Event | Action |
|-------|--------|
| Signup | Send Email 1; schedule Email 2 +24h |
| Aha action completed | Cancel Email 2 |
| Upgraded | Cancel Email 5 |
| Login after Email 6 | Mark win-back succeeded |

Resend API (Node.js):
resend.emails.send({ from: 'you@[DOMAIN]', to: user.email, subject: '...', html: '...' })

Use Inngest, Trigger.dev, or a job queue for scheduling.
