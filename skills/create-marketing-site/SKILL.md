---
description: |
  Builds a fully responsive, multilingual marketing website. Use when the user says "create my marketing site," "build my landing page," "make my homepage," "set up my public website," or when launch-webapp reaches the marketing site step. Generates: hero, features, pricing, blog, free tools section, and footer â all multilingual (next-intl) and mobile-first. Built on Next.js + Tailwind + shadcn/ui to match the webapp stack.
---

# Create marketing site

## Stack
Same as webapp: Next.js 14 App Router + Tailwind + shadcn/ui + next-intl.

## What to collect
- Product name, tagline, one-liner
- Target audience (specific)
- Pricing tiers (names + prices + features per tier)
- Languages needed
- Free tools to include
- Blog? (MDX)
- Logo/brand?

## Pages
/ | /pricing | /blog | /blog/[slug] | /tools | /tools/[slug] | /about | /changelog | /privacy | /terms

## Homepage sections (as standalone components in /components/marketing/)

1. **Hero**: H1 value prop (benefit), subheadline, primary + secondary CTA, social proof strip, hero image
2. **Problem â Solution**: "If you've ever struggled with [X]...", 3-column pain points â solutions
3. **Features**: 3â6 features, alternating image/text layout, icon + title + outcome
4. **Social Proof**: 3 testimonials + logo wall
5. **Pricing**: 2â3 tiers, monthly/annual toggle, feature comparison table
6. **Free Tools Hub**ê (see below)
7. **FAQ**: 6â8 questions, accordion
8. **Final CTA**: "Ready to [outcome]?" + trust signals
9. **Footer**: logo, nav links, social icons, language switcher, copyright

## Free tools section

Each tool is a standalone page at /tools/[slug]:
- Works without signup (gate results export behind signup)
- All computation in-browser (no API needed)
- Results encodable in URL params
- Schema markup (WebApplication type)
- CTA at bottom: "[Product] does this automatically â sign up free"

Tools hub page (/tools): one row per tool, no images, speed over style.

## Multilingual

Generate translation files for all requested languages (real translations, not placeholders).
Language switcher in nav, alternates in metadata.

## SEO metadata
Every page exports generateMetadata with: title, description, openGraph, twitter:summary_large_image, alternates (multilingual canonicals).¢
