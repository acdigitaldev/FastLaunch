---
description: |
  Sets up technical infrastructure for a new webapp. Use when the user says "set up Supabase," "create my GitHub repo," "set up the backend," "initialize infrastructure," or when the launch-webapp skill reaches step 1. Requires Supabase MCP and GitHub MCP. Creates Supabase project, configures auth, sets up GitHub repo with README and CI workflow.
---

# Infrastructure setup

## Required connectors
- Supabase MCP (database + auth)
- GitHub MCP (repo + CI)

If either is missing, tell the user which one and how to add it, then stop.

## Collect if not already known
- Product name
- Tech stack (to determine CI workflow template)
- Whether to enable Row Level Security (recommend yes)

## Supabase setup
1. Create a new Supabase project (kebab-case name)
2. Show the project URL and anon key
3. Enable email auth
4. Create a profiles table linked to auth.users: id, full_name, avatar_url, updated_at
5. Enable Row Level Security on profiles
6. Add policy: users can only read/update their own profile
7. Display the SQL used for the user to audit

Tell the user to save their service role key from Settings > API.

## GitHub setup
1. Create a new private repo (kebab-case name)
2. Initialize with README: product name, description, tech stack, getting started
3. Add .gitignore for their stack
4. Create .github/workflows/ci.yml with lint + test workflow

## Summary output

Infrastructure ready

Supabase
  Project URL: [url]
  Anon key: [key] (safe for frontend)
  Save your service role key from Supabase dashboard > Settings > API

GitHub
  Repo: [url]
  Clone: git clone [url]
