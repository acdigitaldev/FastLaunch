# FastLaunch

> The complete solopreneur webapp launch kit â from zero to live in one Claude session.

Built by [Alexandre Contador](https://github.com/acdigitaldev) Â· Powered by Rankahead + WhatsChat

---

## What it does

FastLaunch is a Claude plugin that automates the full launch workflow for solopreneurs building webapps. Instead of spending days (or weeks) setting up infra, billing, email, SEO, analytics, Meta Pixel, social accounts, and content manually -- you run one command and Claude handles it all.

### The full sequence (20 steps)

| # | Step | What happens | Connector |
|---|------|------------|-----------|
| 1 | Domain & DNS | Find domain, guide Namecheap purchase, configure DNS | Namecheap (via browser) |
| 2 | Supabase + GitHub | Create Supabase project + GitHub repo | Supabase MCP, GitHub MCP |
| 3 | Web app scaffold | Next.js + Tailwind + shadcn, i18n, workspaces, user admin | -- |
| 4 | Marketing site | Responsive, multilingual, free tools, blog | -- |
| 5 | Deployment | Vercel + GitHub auto-deploy, env vars, SSL | Vercel MCP |
| 6 | Stripe billing | Products, checkout, customer portal, webhooks | Stripe |
| 7 | Email auth | SPF, DKIM, DMARC -- emails land in inbox, not spam | Resend MCP |
| 8 | Onboarding emails | 6-email PLG sequence | Resend MCP |
| 9 | Analytics | GA4 property, GSC, robots.txt, GTM, core events | Browser |
| 10 | Meta Pixel | Meta Business Manager, Pixel setup, events, ad account | Browser |
| 11 | GDPR | Cookie consent, privacy policy, terms of service | -- |
| 12 | SEO | Domain setup, competitors, persona, first blog post | Rankahead MCP |
| 13 | CRM | HubSpot, Notion, or Pipedrive/Attio | HubSpot / Notion MCP |
| 14 | Social accounts | LinkedIn company page, Reddit account + subreddits | Browser |
| 15 | WhatsApp bot | Copy + setup guide | WhatsChat (guided) |
| 16 | YouTube channel | Channel, branding, first 3 video concepts | Browser |
| 17 | Launch posts | 12 posts across 4 platforms | -- |
| 18 | Content plan | 3-month calendar | -- |
| 19 | Error monitoring | Sentry setup for client + server errors | -- |

---

## Quick start

### 1. Install the plugin

In Claude Desktop (Cowork mode): Settings -> Plugins -> Install -> select the `.plugin` file from [Releases](https://github.com/acdigitaldev/FastLaunch/releases).

### 2. Add your connectors

| Connector | Where to get it |
|-----------|----------------|
| `SUPABASE_ACCESS_TOKEN` | supabase.com -> Account -> Access Tokens |
| `GITHUB_PERSONAL_ACCESS_TOKEN` | github.com -> Settings -> Developer settings -> PAT |
| `RESEND_API_KEY` | resend.com -> API Keys |
| `RANKAHEAD_API_KEY` | rankahead.com -> Settings -> API |

### 3. Launch

Say: **"Launch my app -- it's called [name] and it [description]"**

---

## Skills included

| Skill | What to say |
|-------|-------------|
| `launch-webapp` | "Launch my app" / "Run the full launch checklist" |
| `create-webapp` | "Create my webapp" |
| `create-marketing-site` | "Build my marketing site" |
| `setup-infra` | "Set up my Supabase and GitHub" |
| `setup-domain` | "Find me a domain" |
| `setup-deployment` | "Deploy to Vercel" |
| `setup-stripe` | "Add billing" |
| `setup-email-auth` | "Set up email authentication" |
| `setup-onboarding-emails` | "Create my onboarding email sequence" |
| `setup-analytics` | "Set up GA4, GSC, and GTM" |
| `setup-meta-pixel` | "Set up Meta Pixel" / "Set up Meta Business Manager" |
| `setup-gdpr` | "Add cookie consent" |
| `setup-rankahead-seo` | "Set up my SEO" |
| `setup-crm` | "Set up my CRM" |
| `setup-social-accounts` | "Set up my LinkedIn and Reddit" |
| `setup-whatsapp` | "Set up my WhatsApp bot" |
| `setup-youtube` | "Create my YouTube channel" |
| `first-posts` | "Write my launch social posts" |
| `content-plan` | "Create my 3-month content plan" |
| `setup-monitoring` | "Set up Sentry" |

---

## Optional tools

**Rankahead** -- SEO + AEO/GEO content on autopilot. [rankahead.com](https://rankahead.com)

**WhatsChat** -- WhatsApp contact button for your site. [whatschat.com](https://whatschat.com)

---

## Token estimate

| Run type | Tokens | Cost (Sonnet) |
|----------|--------|----------------|
| Minimal viable launch (8 steps) | ~40,000-70,000 | ~$0.10-$0.20 |
| Full launch (all 20 steps) | ~80,000-140,000 | ~$0.20-$0.40 |

---

## Tech stack generated

- **Framework**: Next.js 14 (App Router)
- **Styling**: Tailwind CSS + shadcn/ui
- **Database + Auth**: Supabase
- **i18n**: next-intl
- **Payments**: Stripe
- **Email**: Resend
- **Deployment**: Vercel
- **Analytics**: GA4 + GTM
- **Paid ads**: Meta Pixel
- **SEO**: Rankahead
- **Errors**: Sentry
- **CRM**: HubSpot / Notion / Pipedrive / Attio

---

## Contributing

PRs welcome:
- New platform support (TikTok, Pinterest, Substack)
- Additional language support
- New MCP connectors
- Better email templates
- Mobile app launch variant

---

## License

MIT -- use it, fork it, sell products with it.
# FastLaunch

> The complete solopreneur webapp launch kit — from zero to live in one Claude session.

Built by [Alexandre Contador](https://github.com/acdigitaldev) · Powered by Rankahead + WhatsChat

---

## What it does

FastLaunch is a Claude plugin that automates the full launch workflow for solopreneurs building webapps. Instead of spending days setting up infra, email sequences, SEO, and social media manually — you run one command and Claude handles it all, pausing for your approval before anything irreversible.

### The full sequence

| Step | What happens | Connector |
|------|-------------|-----------|
| 1 | Supabase project + GitHub repo | Supabase MCP, GitHub MCP |
| 2 | 6-email PLG onboarding sequence | Resend MCP |
| 3 | SEO setup + first blog post | Rankahead MCP |
| 4 | 12 launch posts across 4 platforms | (generated by Claude) |
| 5 | WhatsApp bot copy + setup guide | WhatsChat (guided) |
| 6 | 3-month content calendar | (generated by Claude) |

---

## Quick start

### 1. Install the plugin

In Claude Desktop (Cowork mode): Settings → Plugins → Install → select the .plugin file from Releases.

### 2. Add your connectors

| Connector | Where to get it |
|-----------|----------------|
| SUPABASE_ACCESS_TOKEN | supabase.com → Account → Access Tokens |
| GITHUB_PERSONAL_ACCESS_TOKEN | github.com → Settings → Developer settings → PAT |
| RESEND_API_KEY | resend.com → API Keys |
| RANKAHEAD_API_KEY | rankahead.com → Settings → API |

### 3. Launch

Say: "Launch my app — it's called [name] and it [description]"

Claude walks you through the full checklist, confirms before each step, and delivers everything.

---

## Skills included

| Skill | What to say |
|-------|-------------|
| launch-webapp | "Launch my app" / "Run the full launch checklist" |
| setup-infra | "Set up my Supabase and GitHub" |
| setup-onboarding-emails | "Create my onboarding email sequence" |
| setup-rankahead-seo | "Set up my SEO with Rankahead" |
| setup-whatsapp | "Set up my WhatsApp bot" |
| first-posts | "Write my launch social posts" |
| content-plan | "Create my 3-month content plan" |

---

## Optional tools

**Rankahead** — SEO + AEO/GEO content on autopilot. Monitors your domain, finds content gaps, auto-generates blog posts. [rankahead.com](https://rankahead.com)

**WhatsChat** — WhatsApp contact button for your site. FastLaunch generates all bot copy and setup instructions. [whatschat.com](https://whatschat.com)

---

## Token estimate

A full launch-webapp run uses approximately 40,000–70,000 tokens. At Claude Sonnet pricing: ~$0.10–$0.20 per full launch.

---

## Contributing

PRs welcome: new platforms (TikTok, YouTube), more languages, new MCP connectors (Stripe, Plausible).

## License

MIT — use it, fork it, sell products with it.
