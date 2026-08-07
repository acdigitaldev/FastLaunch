---
description: |
  Sets up professional social media accounts with brand-consistent descriptions, cover images, and first-post strategy. Use when the user says "set up my social accounts," "create my LinkedIn company page," "set up Reddit," "set up my social profiles," or when launch-webapp reaches the social accounts step. Covers LinkedIn company page, Reddit account + subreddits, and social profile copy for all channels. Integrated with the SEO package. Guides through design assets needed.
---

# Social accounts setup

## What to collect

- Product name and tagline
- Target audience (specific â this shapes subreddit selection and tone)
- Founder's personal LinkedIn URL (needed to create LinkedIn company page)
- Brand: logo file, primary color, cover image size preferences
- Languages (company descriptions may need localization)

---

## LinkedIn Company Page

### Why it matters for SEO
LinkedIn company pages rank in Google for brand name searches. A complete profile with keyword-rich description improves brand authority and shows up in AI search results.

### Setup steps (guided via Claude in Chrome)

1. Go to linkedin.com/company/setup/new
2. Company name: `[Product Name]`
3. LinkedIn public URL: `linkedin.com/company/[slug]` â keep it clean, match domain slug
4. Website: `https://[domain]`
5. Industry: select the most relevant category
6. Company size: 1â10 employees (honest)
7. Company type: Self-employed or Privately held

### Profile copy to generate

**Tagline** (120 chars max):
```
[One-line value prop focused on the outcome, not the feature]
```

**About section** (2,000 chars max â generate the full thing):
```
[Company name] helps [specific audience] [achieve outcome] without [old way of doing it].

[2-sentence expansion of the problem]

[What you built and why â founder story in 2â3 sentences]

Our product:
â [Feature 1 as benefit]
â [Feature 2 as benefit]
â [Feature 3 as benefit]

Used by [X type of people] in [X countries / X companies].

[Website URL] | Get started free in [X minutes]

Keywords: [relevant keywords for the industry, comma-separated â these help LinkedIn search ranking]
```

**Specialties** (shown on profile, searchable):
List 5â10 comma-separated keywords relevant to the product and industry.

### Cover image specs
- Size: 1128 Ã 191 px
- Content: product screenshot or value prop with brand colors
- Generate a Canva-ready brief: `[Color] background, product name in [font weight], tagline below, product mockup on right`

### First 3 LinkedIn posts (company page)
Generate:
1. **Launch post** â  "We're live. Here's what we built and why."
2. **Problem post** - "The painful truth about [industry problem]"
3. **Customer story** - "How [type of user] achieved [result] with [product]"

### LinkedIn Insight Tag (for remarketing)
After creating the page:
1. Campaign Manager â Account Assets â Insight Tag
2. Copy the tag script
3. Add to GTM as a new tag â trigger: All Pages

---

## Reddit Account + Subreddit Strategy

### Why Reddit for SEO
Reddit content ranks aggressively for "best X", "X vs Y", and "how to Y" queries. A genuine presence in relevant subreddits drives long-tail organic traffic and builds social proof.

### Account setup (guided via Claude in Chrome)

1. Create a Reddit account at reddit.com/register
2. Username: use founder's name or a neutral handle (NOT the product name - Reddit hates corporate accounts)
3. Complete profile: avatar, bio mentioning you're a founder working on [product]
4. Verify email
5. Join the subreddits below before posting anything

### Subreddit research

Based on the product description, identify:

**Tier 1 - Core audience subreddits** (where target users hang out):
Use web search: "reddit [audience type] community" + "reddit [problem] community"

**Tier 2 - Founder/builder subreddits** (for build-in-public content):
- r/SaaS
- r/indiehackers
- r/startups
- r/EntrepreneurRideAlong
- r/microsaas

**Tier 3 - Tools/productivity** (broader reach):
- r/productivity
- r/nocode (if relevant)
- r/webdev (if relevant)

Generate a table:
| Subreddit | Members | Best content type | Post frequency |
|-----------|---------|------------------|-----------------|
| ... | ... | ... | ... |

### Reddit content strategy

**Rule #1**: Never promote directly for at least 30 days. Build karma first by being genuinely helpful.

**Month 1 - Karma building**:
- Answer 3 questions per week in core subreddits (no links, just help)
- Share learnings without pitching (e.g., "I built X and here's what I learned about Y")

**Month 2 - Soft mentions**:
- Answer questions where the product naturally helps, mention it transparently: "I built a tool for this, happy to share if useful"
- Share case studies with results (not sales pitches)

**Month 3 - Content drops**:
- Post long-form "I built X in public, here are the numbers" posts
- Share free tools from the marketing site - these perform extremely well on Reddit

### First Reddit post to draft (r/indiehackers or r/SaaS)

Use the reddit-post-drafter skill for the full post. Angle options:
- "I quit [job/situation] to build [product]. [X] months later, here's what happened"
- "I spent [X] hours solving [problem]. Here's everything I learned (and the tool I built)"
- "Sharing our [first metric] - what worked and what didn't"

### Reddit SEO play
Submit free tools to relevant subreddits with a post that teaches something (not just "check out my tool"). Reddit posts with high engagement rank for competitive keywords within days.

---

## Other social profiles - copy to generate

For ach platform the user is on (Instagram, Twitter/X, Tdst/Threads):

**Bio copy** (generate per platform, respecting character limits):

| Platform | Char limit | Format |
|----------|-----------|--------|
| Instagram | 150 | Line breaks, emoji ok, link in bio |
| Twitter/X | 160 | One punchy line + URL |
| Threads | 150 | Casual, personality-forward |
| YouTube | 1,000 | Full description, keywords, links |

**Template for each bio**:
```
[What you do in 5 words]
[Who it's for]
[Proof or credibility signal]
[CTA + link]
```

---

## Profile image + cover consistency

Generate a brief for design (use logo-creator or Canva):

```
Profile picture: Logo mark on [brand color] circle, 500Ã500px
Cover/banner:
  - LinkedIn: 1128Ã191px - product screenshot + "tagline" text overlay
  - Twitter/X: 1500Ã500px - same style, different crop
  - YouTube: 2560Ã1440px - channel name + content promise ("New videos every [X]")
  - Reddit: avatar only, keep founder-personal (not a logo)
```

---

## SEO package integration

After accounts are live:
1. Add social profile links to homepage footer + schema markup (`sameAs` in Organization schema)
2. Add LinkedIn company page URL to Google Search Console for brand monitoring
3. Submit social profiles to Google: search "[product name]" - if Knowledge Panel appears, claim it
4. Add social sharing buttons to blog posts + free tools pages

---

## After setup

- "Write the first social posts?" -> invoke first-posts skill
- "Create a 3-month content plan?" -> invoke content-plan skill
- "Set up your YouTube channel?" -> invoke setup-youtube skill
