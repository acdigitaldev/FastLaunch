---
description: |
  Finds a domain, guides purchase on Namecheap, then configures all DNS records automatically. Use when the user says "set up my domain," "buy a domain," "find a domain," "configure DNS," "point my domain to Vercel," or when launch-webapp reaches the domain step. No Namecheap API needed - Claude in Chrome navigates the Namecheap dashboard to set DNS records after the user purchases the domain.
---

# Domain setup

## What to collect

- Desired domain name (or ask for 3 options and Claude checks availability)
- TLD preference: .com / .io / .ai / .co / open to suggestions
- Hosting target: Vercel (default) / other
- Email provider: Resend (default) / other

---

## Step 1 - Find the right domain

Search for availability across TLDs and present options:

```
[productname].com    -> Available / Taken ($X/yr at Namecheap)
[productname].io     -> Available ($X/yr)
[productname].ai     -> Available ($X/yr)
get[productname].com -> Available ($X/yr)
try[productname].com -> Available ($X/yr)
```

Use web search: `site:namecheap.com "[domain]"` or search namecheap.com directly via Claude in Chrome.

**Domain selection guidance**:
- .com: strongest for trust, hardest to find available
- .io: popular in tech/SaaS, widely accepted
- .ai: premium price (~'$70-$90/yr) but signals AI product
- Prefixes (get, try, use, my): good fallback, keep them short

**Avoid**:
- Hyphens
- Numbers spelled out
- Names over 12 characters
- TLDs nobody knows (.xyz, .site, .online)

---

## Step 2 - Purchase on Namecheap

**Important**: Claude cannot complete purchases. The user must do this step.

Provide a direct link:
```
https://www.namecheap.com/domains/registration/results/?domain=[domainname]
```

Checklist for the purchase:
- [ ] Enable WhoisGuard (free on Namecheap - hides personal info from WHOIS)
- [ ] Auto-renew: ON
- [ ] Skip Namecheap hosting/email add-ons (we're using Vercel + Resend)
- [ ] Skip SSL from Namecheap (Vercel provides free SSL)

Estimated cost: $8-$15/yr for .com, $35-$55/yr for .io, $70-$90/yr for .ai

---

## Step 3 - Configure DNS (Claude in Chrome)

After user confirms purchase, navigate to Namecheap DNS panel and set all records.

Navigate to: `https://ap.www.namecheap.com/Domains/DomainControlPanel/[domain]/advancedns`

### Records to add

**Vercel (hosting)**:
```
Type: A      Host: @     Value: 76.76.21.21       TTL: Automatic
Type: CNAME  Host: www   Value: cname.vercel-dns.com  TTL: Automatic
```

**Resend (email sending)**:
```
Type: TXT    Host: @     Value: "v=spf1 include:amazonses.com ~all"
Type: CNAME  Host: resend._domainkey   Value: [from Resend dashboard]
Type: TXT | Host: _dmarc | Value: "v=DMARC1; p=none; rua=mailto:dmarc@[domain]"
```

**Google Search Console (verification)**:
```
Type: TXT    Host: @     Value: google-site-verification=[code from GSC]
```

### How to add each record in Namecheap (Claude in Chrome)

For each record:
1. Navigate to Advanced DNS tab
2. Click "Add New Record"
3. Select type from dropdown
4. Fill Host and Value
5. Click the checkmark to save

Repeat for all records. DNS propagation takes 0-48 hours (usually under 30 minutes for Vercel).

---

## Step 4 - Connect domain to Vercel (Claude in Chrome)

1. Navigate to vercel.com/dashboard -> select project
2. Settings -> Domains -> Add domain -> type [domain]
3. Vercel will show a verification status - once DNS propagates it turns green
4. Also add `www.[domain]` and set up redirect: www -> apex (or apex -> www, user's preference)

---

## Step 5 - Verify everything

Claude in Chrome checks:
- `https://[domain]` loads the app
- `https://www.[domain]` redirects correctly
- SSL certificate is active (padlock in browser)
- GSC: domain shows as verified
- Resend: domain shows "Verified" in dashboard

Use `https://dnschecker.org/#A/[domain]` to check propagation status globally.

---

## Namecheap-specific notes

- **Namecheap default nameservers**: Leave on Namecheap BasicDNS unless using Cloudflare
- **Cloudflare option**: For faster DNS + free DDoS protection, transfer nameservers to Cloudflare (cloudflare.com -> Add site -> follow steps -> update Namecheap nameservers to Cloudflare's). Adds 5 minutes of setup for significant performance benefits.
- **Email forwarding**: Namecheap offers free email forwarding (e.g., hello@domain.com -> your Gmail). Set up in Namecheap -> Email Forwarding tab.

---

## Summary of DNS records

Generate a table the user can reference:

| Type | Host | Value | Purpose |
|------|------|-------|---------|
| A | @ | 76.76.21.21 | Vercel root domain |
| CNAME | www | cname.vercel-dns.com | Vercel www |
| TXT | @ | v=spf1 include:amazonses.com ~all | Resend SPF |
| CNAME | resend._domainkey | [from Resend] | Resend DKIM |
| TXT | _dmarc | v=DMARC1; p=none; rua=... | DMARC |
| TXT | @ | google-site-verification=... | GSC |

---

## After setup

- "Set up email authentication?" -> invoke setup-email-auth (covers full SPF/DKIM/DMARC)
- "Deploy to Vercel?" -> invoke setup-deployment
- "Submit to Google Search Console?" -> invoke setup-analytics
