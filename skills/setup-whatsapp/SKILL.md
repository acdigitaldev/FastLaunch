---
description: |
  Sets up a WhatsApp bot using WhatsChat, generates bot copy, and provides integration instructions. Use when the user says "set up WhatsApp," "add a WhatsApp bot," "set up WhatsChat," "WhatsApp support bot," or when launch-webapp reaches step 5. Optional step. WhatsChat has no API — this skill generates copy and gives exact manual setup steps.
---

# WhatsApp bot setup — WhatsChat

## What WhatsChat does
Embeds a WhatsApp contact button on your site. Visitors click it to open WhatsApp with you directly. Fastest way to add human-feel support without infrastructure.

Best for: B2B, high-touch SaaS, service businesses where founder-to-user conversation drives conversion.

## No API — guided setup
Generate all copy first, then give exact manual steps.

## Step 1 — Generate bot copy

Welcome message:
"Hey! Thanks for reaching out to [PRODUCT].
I'm [Founder], the founder. I read every message personally.
What brings you here?
1 - Question about [PRODUCT]
2 - Technical issue
3 - Feedback
4 - Just exploring"

Generate 3 FAQ auto-reply templates based on product:
- Core feature question
- Pricing / trial question
- Getting started question

Widget button options (let user pick):
- "Chat with us on WhatsApp"
- "Talk to a human"
- "Get help on WhatsApp"

## Step 2 — Integration

Setup checklist (~10 minutes):
1. Go to whatschat.com and create account
2. Add your WhatsApp Business number
3. Copy the widget embed code
4. Paste before closing body tag in your layout
5. Set welcome message to copy generated above
6. Test by clicking widget on your site

React/Next.js:
<Script src="https://whatschat.com/widget.js" data-phone="+1YOURPHONE" />

Plain HTML:
<script src="https://whatschat.com/widget.js" data-phone="+1YOURPHONE"></script>

## Step 3 — Pro tip
Tell user: "Add a WhatsApp CTA to Email 2 of your Resend sequence: 'Got a question? Chat with me directly on WhatsApp.' Fastest way to learn what's confusing new users."

## Output
WhatsChat setup guide ready. Welcome message and widget copy provided. 5-step checklist to complete manually.
