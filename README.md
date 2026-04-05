# EmDash Blog Team — Paperclip Agent Company

A ready-to-import [Paperclip](https://paperclip.ai) agent company that runs your [emdash CMS](https://github.com/emdash-cms/emdash) blog. Four agents handle the full lifecycle: planning what to write, writing it, optimizing for search, and promoting it.

## The team

```
CEO (strategic direction)
└── Website Manager (editorial lead)
    ├── Content Writer      — drafts and edits posts in Portable Text
    ├── SEO Specialist      — meta tags, slugs, redirects, 404 monitoring
    └── Distribution Manager — social media, newsletters, messaging
```

| Agent | What it does | Skills |
|-------|-------------|--------|
| **CEO** | Sets strategic direction, defines content pillars, approves consequential decisions | emdash, content-strategy |
| **Website Manager** | Maintains content calendar, reviews drafts, coordinates the team, makes publish/schedule decisions | emdash, content-strategy |
| **Content Writer** | Writes blog posts, formats content in Portable Text, uploads images, creates drafts | emdash, copywriting, copy-editing |
| **SEO Specialist** | Optimizes meta tags, manages slugs and redirects, monitors 404s, audits existing content | emdash, seo-audit, schema-markup, site-architecture |
| **Distribution Manager** | Promotes published posts across social channels, drafts newsletter content, distributes via available tools | emdash, social-content |

### Teams

- **Editorial** — Website Manager + Content Writer + SEO Specialist (plan → write → optimize → publish)
- **Growth** — SEO Specialist + Distribution Manager (organic discovery + active promotion)

## Quick start

### 1. Prerequisites

- A running [Paperclip](https://paperclip.ai) instance
- An [emdash](https://github.com/emdash-cms/emdash) blog deployed on Cloudflare (or self-hosted)
- An emdash Personal Access Token (PAT) — see [Getting a token](#getting-an-emdash-api-token) below

### 2. Clone the company

```bash
cd ~/.paperclip/local-companies
git clone https://github.com/ScaleLeanChris/emdash-blog-team.git
```

### 3. Import into Paperclip

```bash
npx paperclipai company import --from ~/.paperclip/local-companies/emdash-blog-team
```

### 4. Configure environment variables

After import, each agent needs the emdash connection details. The `.paperclip.yaml` file declares two required inputs per agent:

| Variable | Kind | Description |
|----------|------|-------------|
| `EMDASH_URL` | config | Base URL of your emdash instance |
| `EMDASH_API_TOKEN` | secret | Personal Access Token (`ec_pat_*`) |

Set these on each agent through the Paperclip UI (agent settings → adapter config → environment variables) or via the Paperclip API. All 5 agents need both variables configured.

### 5. Verify

Trigger a heartbeat on the CEO agent. On first run, the CEO will:
1. Verify the emdash connection using MCP tools
2. Create a company goal and project
3. Create onboarding tasks for each team member

If the CEO reports it's blocked, check that the env vars are set correctly.

## Getting an emdash API token

1. Go to your emdash admin panel: `https://your-blog.example.com/_emdash/admin`
2. Navigate to **Settings → API Tokens**
3. Click **Create Token**
4. Name it (e.g., "Paperclip Blog Team")
5. Select these scopes:

| Scope | Required | Used by |
|-------|----------|---------|
| `content:read` | Yes | All agents — reading posts, search |
| `content:write` | Yes | All agents — creating, editing, publishing |
| `media:read` | Yes | Content Writer — browsing media library |
| `media:write` | Yes | Content Writer — uploading images |
| `schema:read` | Recommended | All agents — discovering field names |

6. Click **Create** and copy the token (`ec_pat_...`) — you'll only see it once

## How the workflow runs

```
CEO sets editorial mission and content pillars
    │
    └── Heartbeat triggers Website Manager
        │
        ├── Audits content inventory (what exists, what's stale, what's missing)
        ├── Plans next batch of content (topics, keywords, deadlines)
    │
    ├── Assigns writing task → Content Writer
    │   ├── Checks schema for field names
    │   ├── Writes post in Portable Text
    │   ├── Uploads images via REST API
    │   └── Creates draft, notifies Website Manager
    │
    ├── Routes draft for SEO → SEO Specialist
    │   ├── Sets meta title + description
    │   ├── Optimizes slug
    │   ├── Adds internal links
    │   └── Assigns taxonomy terms
    │
    ├── Website Manager publishes or schedules
    │
    └── Triggers promotion → Distribution Manager
        ├── Crafts platform-specific social posts
        ├── Posts to configured social and messaging channels
        └── Drafts newsletter content
```

## What's included

All skills are bundled in this package — no external dependencies needed:

```
emdash-blog-team/
├── COMPANY.md                    # Company metadata and goals
├── README.md                     # This file
├── paperclip.manifest.json       # Import manifest (agents + skills)
├── agents/
│   ├── ceo/AGENTS.md
│   ├── website-manager/AGENTS.md
│   ├── content-writer/AGENTS.md
│   ├── seo-specialist/AGENTS.md
│   └── distribution-manager/AGENTS.md
├── teams/
│   ├── editorial/TEAM.md
│   └── growth/TEAM.md
└── skills/                       # All 8 skills bundled
    ├── emdash/                   #   Core CMS skill (25 reference files)
    ├── content-strategy/         #   Editorial planning
    ├── copywriting/              #   Writing craft
    ├── copy-editing/             #   Editing and polish
    ├── seo-audit/                #   Technical SEO diagnosis
    ├── schema-markup/            #   Structured data
    ├── site-architecture/        #   Information hierarchy
    └── social-content/           #   Platform-specific social posts
```

## Customization

### Change the team size

The four-agent structure is a starting point. You can:

- **Merge roles:** Assign the SEO Specialist's skills to the Content Writer for a 3-agent team
- **Add agents:** Create an Analytics agent or a separate Editor for review workflows
- **Split roles:** Create multiple Content Writers for higher cadence

Edit the `agents/` directory and update `paperclip.manifest.json` accordingly.

### Use with a different CMS

The `emdash` skill is specific to emdash CMS. The other seven skills (content-strategy, copywriting, etc.) are CMS-agnostic and work with any content platform.

### Connect distribution channels

The Distribution Manager uses whatever social and messaging tools are available in the agent's environment. Configure your preferred distribution channels (social media APIs, messaging platforms, etc.) separately in your Paperclip instance.

## Environment variables reference

| Variable | Kind | Description |
|----------|------|-------------|
| `EMDASH_URL` | config | Base URL of your emdash instance |
| `EMDASH_API_TOKEN` | secret | Personal Access Token (`ec_pat_*`) |

Set through the Paperclip UI on each agent's adapter config. Values are encrypted at rest.

## Updating

Skills and agent definitions are not auto-updated after import. To get the latest version:

```bash
cd ~/.paperclip/local-companies/emdash-blog-team
git pull
npx paperclipai company import --from ~/.paperclip/local-companies/emdash-blog-team --collision replace
```

This pulls the latest changes and re-imports. Your agent environment variables and task history are preserved — only skill content and agent definitions are replaced.

## Related

- [emdash-paperclip-skill](https://github.com/ScaleLeanChris/emdash-paperclip-skill) — The standalone emdash skill (use this if you want to add emdash capabilities to your own agent team)
- [emdash CMS](https://github.com/emdash-cms/emdash) — The CMS itself
- [Paperclip](https://paperclip.ai) — Multi-agent coordination platform
- [paperclip.space](https://paperclip.space) — Paperclip community hub, where this package is listed

## License

MIT
