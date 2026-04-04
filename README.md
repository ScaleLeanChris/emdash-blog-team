# EmDash Blog Team — Paperclip Agent Company

A ready-to-import [Paperclip](https://paperclip.ai) agent company that runs your [emdash CMS](https://github.com/emdash-cms/emdash) blog. Four agents handle the full lifecycle: planning what to write, writing it, optimizing for search, and promoting it.

## The team

```
CEO (strategic direction)
└── Blog Manager (editorial lead)
    ├── Content Writer      — drafts and edits posts in Portable Text
    ├── SEO Specialist      — meta tags, slugs, redirects, 404 monitoring
    └── Distribution Manager — social media, newsletters, Hermes messaging
```

| Agent | What it does | Skills |
|-------|-------------|--------|
| **CEO** | Sets strategic direction, defines content pillars, approves consequential decisions | emdash, content-strategy |
| **Blog Manager** | Maintains content calendar, reviews drafts, coordinates the team, makes publish/schedule decisions | emdash, content-strategy |
| **Content Writer** | Writes blog posts, formats content in Portable Text, uploads images, creates drafts | emdash, copywriting, copy-editing |
| **SEO Specialist** | Optimizes meta tags, manages slugs and redirects, monitors 404s, audits existing content | emdash, seo-audit, schema-markup, site-architecture |
| **Distribution Manager** | Promotes published posts across social channels, drafts newsletter content, distributes via Hermes | emdash, social-content |

### Teams

- **Editorial** — Blog Manager + Content Writer + SEO Specialist (plan → write → optimize → publish)
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

After import, each agent needs the emdash connection details. Set them via the Paperclip API:

```bash
# List agents to get their IDs
curl http://127.0.0.1:3100/api/companies/{companyId}/agents

# Configure each agent (repeat for all 4 agents)
curl -X PATCH "http://127.0.0.1:3100/api/agents/{agentId}" \
  -H "Content-Type: application/json" \
  -d '{
    "adapterConfig": {
      "env": {
        "EMDASH_URL": "https://your-blog.example.com",
        "EMDASH_API_TOKEN": "ec_pat_your_token_here"
      }
    }
  }'
```

**Important:** There is no UI for agent environment variables yet. You must use the REST API as shown above. The values are stored encrypted and injected into each agent's process at runtime.

#### Bulk configuration script

To configure all four agents at once:

```bash
COMPANY_ID="your-company-id"
EMDASH_URL="https://your-blog.example.com"
EMDASH_TOKEN="ec_pat_your_token_here"
API="http://127.0.0.1:3100"

# Get all agent IDs for this company
AGENT_IDS=$(curl -s "$API/api/companies/$COMPANY_ID/agents" | python3 -c "
import sys, json
for a in json.load(sys.stdin)['data']:
    print(a['id'])
")

# Configure each agent
for ID in $AGENT_IDS; do
  curl -s -X PATCH "$API/api/agents/$ID" \
    -H "Content-Type: application/json" \
    -d "{
      \"adapterConfig\": {
        \"env\": {
          \"EMDASH_URL\": \"$EMDASH_URL\",
          \"EMDASH_API_TOKEN\": \"$EMDASH_TOKEN\"
        }
      }
    }"
  echo " → Configured $ID"
done
```

### 5. Verify

Check that agents can reach emdash:

```bash
curl -s -H "Authorization: Bearer $EMDASH_TOKEN" \
  "$EMDASH_URL/_emdash/api/auth/me"
```

You should see your user info with role level 40+ (EDITOR or ADMIN).

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
    └── Heartbeat triggers Blog Manager
        │
        ├── Audits content inventory (what exists, what's stale, what's missing)
        ├── Plans next batch of content (topics, keywords, deadlines)
    │
    ├── Assigns writing task → Content Writer
    │   ├── Checks schema for field names
    │   ├── Writes post in Portable Text
    │   ├── Uploads images via REST API
    │   └── Creates draft, notifies Blog Manager
    │
    ├── Routes draft for SEO → SEO Specialist
    │   ├── Sets meta title + description
    │   ├── Optimizes slug
    │   ├── Adds internal links
    │   └── Assigns taxonomy terms
    │
    ├── Blog Manager publishes or schedules
    │
    └── Triggers promotion → Distribution Manager
        ├── Crafts platform-specific social posts
        ├── Sends via Hermes to connected channels
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
│   ├── blog-manager/AGENTS.md
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

The Distribution Manager uses [Hermes](https://github.com/NousResearch/hermes) for multi-channel messaging (Telegram, Discord, Slack, etc.). Configure Hermes separately in your Paperclip instance to enable channel distribution.

## Environment variables reference

| Variable | Where to set | Description |
|----------|-------------|-------------|
| `EMDASH_URL` | Agent `adapterConfig.env` | Base URL of your emdash instance |
| `EMDASH_API_TOKEN` | Agent `adapterConfig.env` | Personal Access Token (`ec_pat_*`) |

Set via `PATCH /api/agents/{agentId}` with `adapterConfig.env` payload. Values are encrypted at rest.

## Related

- [emdash-paperclip-skill](https://github.com/ScaleLeanChris/emdash-paperclip-skill) — The standalone emdash skill (use this if you want to add emdash capabilities to your own agent team)
- [emdash CMS](https://github.com/emdash-cms/emdash) — The CMS itself
- [Paperclip](https://paperclip.ai) — Multi-agent coordination platform

## License

MIT
