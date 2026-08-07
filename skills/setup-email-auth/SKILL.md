---
description: |
  Configures email authentication records (SPF, DKIM, DMARC) to ensure emails sent via Resend land in inbox, not spam. Use when the user says "set up email authentication," "set up SPF DKIM DMARC," "emails going to spam," "configure email deliverability," "set up Resend domain," or when launch-webapp reaches the email authentication step. Uses Claude in Chrome to configure DNS records in Namecheap and verify domain in Resend.
---

# Email authentication setup

## Why this matters

Without SPF, DKIM, and DMARC, transactional emails (onboarding sequences, password resets, invoices) go straight to spam. Gmail and Outlook now reject bulk email from unauthenticated senders. This takes 15 minutes and prevents a major deliverability problem.

---

## What to collect

- Domain (e.g., myproduct.com)
- Resend account (if none, guide creation at resend.com)
- DNS provider (Namecheap by default)

---

## Step 1 â Add domain to Resend (Claude in Chrome)

1. Navigate to resend.com/domains
2. Add Domain â enter `[domain]`
3. Resend shows the required DNS records â copy all of them

Resend will provide:
```
TXT   @              "v=spf1 include:amazonses.com ~all"
CNAME resend._domainkey  [unique DKIM value]
TXT   _dmarc         "v=DMARC1; p=none; rua=mailto:..."
```

---

## Step 2 â Add records in Namecheap (Claude in Chrome)

Navigate to: `https://ap.www.namecheap.com/Domains/DomainControlPanel/[domain]/advancedns`

### SPF record

```
Type: TXT
Host: @
Value: v=spf1 include:amazonses.com ~all
TTL: Automatic
```

If an SPF record already exists (e.g., from Google Workspace), merge them â don't add a second SPF record:
```
v=spf1 include:_spf.google.com include:amazonses.com ~all
```

### DKIM record

```
Type: CNAME
Host: resend._domainkey
Value: [value from Resend dashboard]
TTL: Automatic
```

### DMARC record

```
Type: TXT
Host: _dmarc
Value: v=DMARC1; p=none; rua=mailto:dmarc-reports@[domain]; ruf=mailto:dmarc-reports@[domain]; fo=1
TTL: Automatic
```

**DMARC policy explained**:
- `p=none` â watch only (start here)
- After 30 days with no issues â change to `p=quarantine`
- After 60 days â change to `p=reject` (strongest protection)

---

## Step 3 â Verify in Resend

1. Back to resend.com/domains
2. Click "Verify" next to the domain
3. Status should change to "Verified" within 0-24 hours

If verification fails:
- Check for typos in CNAME value
- Wait 30 minutes for DNS propagation
- Use https://mxtoolbox.com/dkim.aspx to debug

---

## Step 4 â Set sending email in Resend

In Resend -> API KerYè¡:
- Create an API key for production
- Add to .env.local as RESEND_API_KEY

Sender addresses:
- use hello@[domain] or noreply@[domain] for transactional email
- use [firstname]@[domain] for onboarding sequences (personal feel)

---

## Step 5 â Test deliverability

Use https://mail-tester.com - paste their test address, send a test email, and it scores your deliverability 1-10. Target: 9+/10.

---

## Step 6 â Monitor DMARC reports

Free DMARC report monitors:
- https://dmarcreports.io (free tier)
- https://postmark.com/tools/dmarc-report-viewer (free)

These send weekly summaries showing which servers are sending email on your behalf.

---

## DNS record summary

| Type | Host | Value | Purpose |
|------|------|-------|---------|
| TXT | @ | v=spf1 include:amazonses.com ~all | SPF - authorize Resend to send |
| CNAME | resend._domainkey | [from Resend] | DKIM - sign outgoing emails |
| TXT | _dmarc | v=DMARC1; p=none; rua=mailto:... | DMARC - policy + reporting |

---

## After setup

- Resend domain verified -> email sequences ready to go live
- "Set up onboarding email sequence?" -> invoke setup-onboarding-emails
- "Move DMARC policy to quarantine?" -> update TXT record, change p=none to p=quarantine
