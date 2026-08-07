---
description: |
  Scaffolds a full-stack webapp using Next.js 14 + Tailwind CSS + shadcn/ui. Use when the user says "create my webapp," "scaffold my app," "build my SaaS app," "set up my Next.js app," or when launch-webapp reaches the webapp creation step. Generates project structure with: internationalization (next-intl), workspace/org model, user admin panel, brand system (design tokens), responsive layout, Supabase auth + RLS, and global settings. Outputs a ready-to-push repo structure.
---

# Create webapp â Next.js + Tailwind + shadcn/ui

## Stack
- **Framework**: Next.js 14 (App Router)
- **Styling**: Tailwind CSS + shadcn/ui
- **i18n**: next-intl (file-based routing: `/en`, `/es`, `/pt`, etc.)
- **Auth + DB**: Supabase (auth, RLS, profiles)
- **State**: Zustand (global) + React Query (server state)
- **Forms**: react-hook-form + zod

---

## What to collect

Before generating anything:
- Product name and slug (used for package name, env prefix)
- Default language + additional languages (e.g., en, es, pt-BR)
- Brand colors (primary, accent, neutral) â if unknown, generate a palette based on the product name
- Features in scope for v1: list them, map each to a route
- Workspace model? (single user / team with members / org with roles)
- Admin panel needed? (user list, impersonation, feature flags)

---

## Project structure to generate

```
/app
  /[locale]                    â next-intl dynamic locale segment
    /(auth) /login /signup /forgot-password
    /(app)
      /dashboard
      /settings /general /workspace /members /billing
      /admin /users /feature-flags (if requested)
/components /ui /layout /workspace /auth
/lib /supabase /stripe /i18n
/messages en.json es.json pt.json
/middleware.ts
```

## Brand system
Generate tailwind.config.ts with CSS variables for brand colors (light + dark mode tokens).

## Internationalization
setup next-intl with locale routing. Generate translation files for all requested languages (not placeholders â real translations).

## Workspace model
Generate Supabase migration: workspaces + workspace_members tables with RLS policies.

## User admin panel (if requested)
/admin/users + /admin/feature-flags, protected by is_admin flag.

## Global settings
workspace_settings table: timezone, locale, notifications jsonb.

## Responsive rules
All layouts mobile-first. Sidebar collapses to bottom nav on mobile. Tables become cards on mobile.

## Scaffolding commands
```bash
npx create-next-app@latest [slug] --typescript --tailwind --app
npx shadcn@latest init && npx shadcn@latest add button card input label dialog table badge avatar dropdown-menu
npm install next-intl zustand @tanstack/react-query react-hook-form zod @supabase/ssr
```

## After generating
- Set up Stripe billing? â invoke setup-stripe
- Domain and deployment? â invoke setup-domain + setup-deployment
- Error monitoring? â invoke setup-monitoring 
