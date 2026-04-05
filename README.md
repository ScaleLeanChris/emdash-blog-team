# EmDash Blog Team — Paperclip Agent Company

A ready-to-import [Paperclip](https://paperclip.ai) agent company that runs your [emdash CMS](https://github.com/emdash-cms/emdash) blog. Four agents handle the full lifecycle: planning what to write, writing it, optimizing for search, and promoting it.

## The team

```
CEO (strategic direction)
└── Website Manager (editorial lead)
    ├── Researcher           — web search and topic research via Tavily
    ├── Content Writer       — drafts and edits posts in Portable Text
    ├── Graphics Designer    — generates featured images and social graphics via Gemini
    ├── SEO Specialist       — meta tags, slugs, redirects, 404 monitoring
    └── Distribution Manager — social media, newsletters, messaging
```

| Agent | What it does | Skills |
|-------|-------------|--------|
| **CEO** | Sets strategic direction, defines content pillars, approves consequential decisions | emdash, content-strategy |
| **Website Manager** | Maintains content calendar, reviews drafts, coordinates the team, makes publish/schedule decisions | emdash, content-strategy |
| **Researcher** | Researches topics, finds sources, analyzes competitors, produces research briefs | tavily-research, emdash |
| **Content Writer** | Writes blog posts from research briefs, formats in Portable Text | emdash, copywriting, copy-editing |
| **SEO Specialist** | Optimizes meta tags, manages slugs and redirects, monitors 404s, audits existing content | emdash, seo-audit, schema-markup, site-architecture |
| **Graphics Designer** | Generates featured images and social graphics using Gemini AI, uploads to emdash media library | gemini-imagegen, emdash |
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

After import, each agent needs the emdash connection details. For **each of the 5 agents**:

1. Go to the agent's page in the Paperclip UI
2. Open **adapter config → environment variables**
3. Add `EMDASH_URL` (your emdash instance URL) and `EMDASH_API_TOKEN` (your PAT)
4. Click **Seal** for the API token (it's a secret)
5. Run **Test Environment** to verify the variables are set

| Variable | Kind | Description |
|----------|------|-------------|
| `EMDASH_URL` | config | Base URL of your emdash instance |
| `EMDASH_API_TOKEN` | secret | Personal Access Token (`ec_pat_*`) — use Seal |

**Troubleshooting:** If the test environment doesn't validate, try toggling the agent's model (e.g., switch from Codex to Claude and back). This forces Paperclip to re-read the adapter config. This is a known workaround.

### 5. Initialize workspaces

The first heartbeat may fail with `"Not inside a trusted directory"` because agent workspaces are not initialized as git repos. If this happens, initialize the workspace manually:

```bash
cd ~/.paperclip/instances/default/workspaces/{agent-id}
git init && git commit --allow-empty -m "Initialize workspace"
```

### 6. First heartbeat

Trigger a heartbeat on the CEO agent. On first run, the CEO will:
1. Verify the emdash connection using MCP tools
2. Create a company goal and project
3. Create onboarding tasks for each team member
4. Post a strategic brief with content pillars and editorial direction

If the CEO reports it's blocked, check that the env vars are set correctly on that agent.

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
│   ├── researcher/AGENTS.md
│   ├── content-writer/AGENTS.md
│   ├── graphics-designer/AGENTS.md
│   ├── seo-specialist/AGENTS.md
│   └── distribution-manager/AGENTS.md
├── teams/
│   ├── editorial/TEAM.md
│   └── growth/TEAM.md
└── skills/                       # All 10 skills bundled
    ├── emdash/                   #   Core CMS skill (25 reference files)
    ├── content-strategy/         #   Editorial planning
    ├── copywriting/              #   Writing craft
    ├── copy-editing/             #   Editing and polish
    ├── seo-audit/                #   Technical SEO diagnosis
    ├── schema-markup/            #   Structured data
    ├── site-architecture/        #   Information hierarchy
    ├── tavily-research/          #   Web search and content research
    ├── gemini-imagegen/          #   AI image generation via Gemini
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

| Variable | Kind | Which agents | Description |
|----------|------|-------------|-------------|
| `EMDASH_URL` | config | All 6 | Base URL of your emdash instance |
| `EMDASH_API_TOKEN` | secret | All 6 | Personal Access Token (`ec_pat_*`) |
| `TAVILY_API_KEY` | secret | Researcher only | Tavily API key (https://app.tavily.com) |
| `DATAFORSEO_LOGIN` | secret | SEO Specialist only | DataForSEO login (https://app.dataforseo.com) |
| `DATAFORSEO_PASSWORD` | secret | SEO Specialist only | DataForSEO password |
| `GEMINI_API_KEY` | secret | Graphics Designer only | Google AI Studio API key (https://aistudio.google.com/apikey) |

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
