---
description: |
  Creates a PLG onboarding email sequence via Resend. Use when the user says "set up onboarding emails," "create my email sequence," "set up Resend," or when launch-webapp reaches step 2. Requires Resend MCP. Templates in references/plg-email-sequence.md.
---

# Onboarding emails — Resend PLG

## Required: Resend MCP

## Collect
- Product name and description
- The aha moment (key first action)
- Verified sending domain

## 6-email sequence

1. Signup (immediate) — Welcome + get to aha moment
2. Day 1 if not activated — Activation nudge
3. Day 3 — Social proof / inspiration
4. Day 7 — Feature discovery
5. Day 14 free users — Upgrade nudge
6. Day 30 inactive — Win-back

Use templates from references/plg-email-sequence.md. Customize per product.
Show subject + preview text before saving. Confirm or batch-approve all.

Behavioral triggers note: tell user that skipping emails based on user behavior requires backend API calls to Resend — offer to write that code.

## Output
List all 6 emails with subjects. Provide audience ID for signup handler wiring.
