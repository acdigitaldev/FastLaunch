---
description: |
  Connects and configures a CRM for the webapp. Use when the user says "set up my CRM," "connect HubSpot," "set up Notion CRM," "connect Pipedrive," "connect Attio," "I need a CRM," or when launch-webapp reaches the CRM step. Covers HubSpot (free CRM), Notion (template-based), and Pipedrive/Attio (sales-focused). Also references open-source GitHub repos for self-hosted CRM options.
---

# CRM setup

## What to collect
- Which CRM? (HubSpot / Notion / Pipedrive / Attio / self-hosted)
- What to track: leads, deals, users, churned customers?
- Existing Supabase users? â sync those to CRM
- Team size: solo or multiple sales people?

## Option A â HubSpot (recommended for B2B SaaS)
Free tier: contacts, deals, email sequences, forms.

Setup: hubspot.com â Create free account â Pipeline: Awareness â Trial â Active â Churned â Custom properties: product_plan, workspace_id, trial_started_at, mrr

Supabase â HubSpot sync:
```ts
const HUBSPOT_API = 'https://api.hubapi.com'
export async function upsertContact(user: { email: string; name: string; plan: string; workspaceId: string }) {
  await fetch(`${HUBSPOT_API}/crm/v3/objects/contacts`, {
    method: 'POST',
    headers: { Authorization: `Bearer ${process.env.HUBSPAT_ACCESS_TOKEN}`, 'Content-Type': 'application/json' },
    body: JSON.stringify({ properties: { email: user.email, firstname: user.name.split(' ')[0], product_plan: user.plan, workspace_id: user.workspaceId } }),
  })
}
```

## Option B â Notion CRM (recommended for solo founders)
Fork: https://github.com/rileydurham23/notion-crm
Or official: notion.so/templates/crm

Contacts db: Name, Email, Company, Status (Lead/Trial/Active/Churned), Plan, MRR, Last contacted
Deals db: Contact (relation), Stage, Value, Close date, Next action
Notion MCP: use notion-create-database to build both automatically.

## Option C â Pipedrive / Attio
**Pipedrive**: Deal-centric pipeline. Set stages: Lead In â Demo Booked â Proposal â Won/Lost. Connect via Zapier or webhook.
**Attio**: PLG-friendly, native Supabase integration. Settings â Integrations â map users, workspaces, set up trial sequences.

## Self-hosted options
| twentyhq/twenty | React+Node+Postgres | Salesforce alternative |
| erxes/erxes | React+Node+MongoDB | CRM+marketing+support |
| Dolibarr/dolibarr | PHP | ERP/CRM for small business |
| SuiteCRM/SuiteCRM | PHP | Open-source Salesforce |

## Track from day 1
contact.created â contact.trial_started â contact.converted â contact.churned â contact.reactivated

## After setup
- Connect Resend email sequences to CRM
- Add lead capture form webhook â CRM
- Import existing Supabase users â Claude writes migration script
