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
