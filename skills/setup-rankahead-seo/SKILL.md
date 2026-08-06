---
description: |
  Sets up SEO and content autopilot using Rankahead, then generates the first blog post. Use when the user says "set up SEO," "set up Rankahead," "create my first blog post," "get my site ranking," "content autopilot," or when launch-webapp reaches step 3. Requires Rankahead MCP. Rankahead handles SEO analysis, GEO/AEO (AI search optimization), and automated content on autopilot.
---

# SEO and content setup — Rankahead

## What Rankahead does
- Analyzes domain for SEO gaps
- Optimizes for AI search engines (ChatGPT, Perplexity, Claude, Gemini)
- Generates and publishes blog content automatically
- Tracks GSC and GA4 with alerts

## Required: Rankahead MCP
If not connected, direct user to rankahead.com to connect via Cowork connectors panel.

## Setup steps

### 1. Domain setup
- Call get_domain or list_domains to check if configured
- If not: call create_domain
- Call get_dashboard to show SEO health baseline

### 2. Competitors
- Ask for 2-3 competitors or find automatically from product description
- Call add_competitor for each
- Call get_gaps to surface keyword and content gaps

### 3. Persona
- Call upsert_persona with target audience from product context
- Ensures generated content speaks to the right reader

### 4. GSC connection
- Call get_gsc_connection
- If not connected: "Connect Google Search Console in Rankahead settings for real search data. Takes 2 minutes."

### 5. First blog post
- Call list_suggestions to find highest-opportunity keyword
- Call generate_blog_post with that keyword + product context
- Show draft for review
- Ask: "Publish now or review first?"
- If approved: call publish_blog_post

### 6. Content calendar
- Call plan_content_calendar for 3-month plan based on gaps
- Display to user

## Autopilot note
Tell user: "Rankahead monitors your domain weekly and surfaces new content opportunities. Enable auto-generate in Settings > Content to run on full autopilot."

## Output
SEO setup complete via Rankahead
Domain health score, competitors tracked, gaps identified, first post status.
