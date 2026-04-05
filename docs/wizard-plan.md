# Plan: EmDash Site Setup Wizard for paperclip.space

## Overview

An interactive onboarding wizard on paperclip.space — styled like gstack's `/office-hours` skill — that walks users through setting up their emdash agent company. Instead of the CEO asking for site direction on first heartbeat (which requires the user to already be inside Paperclip), this wizard captures everything upfront and generates a pre-configured company package.

## The Problem

Right now the onboarding flow is:
1. User imports company package
2. User sets env vars on each agent (clunky)
3. CEO runs first heartbeat, creates a board issue asking "what is your site about?"
4. User answers in Paperclip
5. CEO finally delegates work

This is too many steps. The wizard front-loads the important decisions so the company imports ready to execute.

## The Wizard Flow

### Screen 1: Site Status
- **Is this a new site or existing?**
  - New → we'll help you set one up
  - Existing → connect to your emdash instance
- If existing: enter your emdash URL + verify connection

### Screen 2: Site Purpose
- **What is this site for?** (select one or more)
  - Blog / thought leadership
  - Product marketing / landing pages
  - Documentation / knowledge base
  - Portfolio / showcase
  - E-commerce / product catalog
  - Community / forum
  - Other (free text)

### Screen 3: Audience
- **Who is your target audience?** (free text, with examples)
  - "Developers building with AI tools"
  - "Small business owners looking for marketing help"
  - "Internal team members who need documentation"

### Screen 4: Content Pillars
- **What topics will you cover?** (add 3-5)
  - Suggest based on site purpose + audience
  - User can edit, add, remove
  - Each pillar gets a one-line description

### Screen 5: Voice & Tone
- **How should the content sound?** (select)
  - Professional / formal
  - Casual / conversational
  - Technical / precise
  - Friendly / approachable
  - Bold / opinionated
- **Examples** shown for each selection

### Screen 6: Publishing Cadence
- **How often?**
  - Daily
  - 3x/week
  - Weekly
  - Biweekly
  - Monthly
- **Best day/time?** (for scheduling)

### Screen 7: Competitors / Inspiration
- **Who are your competitors or inspiration?** (optional, add URLs)
  - Used for content gap analysis and differentiation
  - SEO Specialist references these during audits

### Screen 8: Distribution Channels
- **Where will you promote content?** (multi-select)
  - Twitter/X
  - LinkedIn
  - Discord
  - Slack
  - Newsletter (which provider?)
  - Reddit
  - Other

### Screen 9: API Connection
- **Enter your emdash credentials**
  - EMDASH_URL
  - EMDASH_API_TOKEN
  - Wizard verifies connection and discovers schema
  - Shows collection names and field types

### Screen 10: Review & Generate
- Summary of all inputs
- Preview the org chart (CEO → Website Manager → specialists)
- **Generate** button creates:
  1. A customized company package with the editorial mission baked into the CEO's onboarding
  2. Pre-configured `.paperclip.yaml` with the connection details
  3. Content pillars pre-loaded as taxonomy terms to create
  4. Distribution channels pre-configured in the Distribution Manager's instructions
  5. A one-click import command

## What Gets Generated

The wizard produces a customized fork of `emdash-blog-team` where:

- `agents/ceo/references/onboarding.md` — editorial mission is pre-filled (no board question needed)
- `agents/ceo/references/editorial-brief.md` — the full strategy doc from wizard inputs
- `agents/website-manager/AGENTS.md` — content pillars mentioned in instructions
- `agents/seo-specialist/AGENTS.md` — competitors listed for reference
- `agents/distribution-manager/AGENTS.md` — channels pre-configured
- `.paperclip.yaml` — env vars pre-declared
- Taxonomy terms to create on first run (from content pillars)

## Technical Implementation

### Option A: Static site generator (simpler)
- Wizard is a multi-step form on paperclip.space (Astro + React)
- Generates a JSON config → downloads a customized zip or creates a GitHub repo
- No backend needed beyond the form logic

### Option B: Server-side generation (richer)
- Wizard hosted on paperclip.space (Cloudflare Workers)
- On submit, forks the emdash-blog-team repo
- Applies customizations via GitHub API
- Returns a one-click import URL

### Recommendation: Option A for MVP
- Wizard is a client-side React form
- Generates a `company-config.json` with all wizard answers
- A script in the company package reads the config and customizes the files
- User downloads zip → `npx paperclipai company import --from ./`

## Relationship to gstack office-hours

The gstack `/office-hours` skill uses a Socratic questioning approach — six forcing questions that expose reality. The wizard follows the same pattern but as a web UI instead of CLI:

| gstack office-hours | EmDash wizard |
|---|---|
| "What demand evidence do you have?" | "Who is your audience?" |
| "What's the status quo?" | "New site or existing?" |
| "Who is desperate for this?" | "What topics will you cover?" |
| "What's the narrowest wedge?" | "What's your publishing cadence?" |
| "What did you observe?" | "Who are your competitors?" |
| "Where does this go?" | "What are your distribution channels?" |

## Timeline Estimate

### Phase 1: Wizard UI (1-2 weeks)
- Multi-step form component
- Input validation
- Summary/preview screen

### Phase 2: Config generation (1 week)
- JSON config schema
- Company package customization script
- Zip download or GitHub repo creation

### Phase 3: One-click import (1 week)
- Integration with `npx paperclipai company import`
- Optional: `npx companies.sh add` support for the paperclipai/companies catalog

### Phase 4: Polish (1 week)
- Design review
- Mobile responsive
- Analytics on wizard completion funnel
