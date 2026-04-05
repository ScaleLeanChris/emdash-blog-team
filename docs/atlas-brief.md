# Project Brief: EmDash Agent Company for Paperclip

**Date:** 2026-04-04
**Status:** MVP complete, testing in progress

## What This Is

We built a Paperclip agent company that manages an emdash CMS website. It's a 5-agent team (CEO, Website Manager, Content Writer, SEO Specialist, Distribution Manager) that handles the full content lifecycle — from planning what to write, through writing, SEO optimization, publishing, and distribution.

This is designed to be **community-shareable** on paperclip.space as the first example of a CMS management agent company.

## Two Repos

1. **Standalone skill** — https://github.com/ScaleLeanChris/emdash-paperclip-skill
   - A single Paperclip agent skill (`emdash`) that teaches any agent to use emdash CMS
   - 25 reference files covering content, media, SEO, taxonomy, strategy, distribution
   - MCP-first with REST API fallback
   - Can be assigned to any agent in any company

2. **Agent company** — https://github.com/ScaleLeanChris/emdash-blog-team
   - 5 agents with bundled skills (the emdash skill + 7 marketing skills)
   - CEO runs onboarding on first heartbeat: creates goal, project, asks board for site direction, then delegates tasks
   - `.paperclip.yaml` declares required env vars (`EMDASH_URL`, `EMDASH_API_TOKEN`)
   - Follows the `agentcompanies/v1` spec from `paperclipai/companies`

## The emdash Instance

- **URL:** https://paperclip-space-emdash-site.odd-hill-1be0.workers.dev/ (temporary, domain TBD)
- **GitHub:** https://github.com/ScaleLeanChris/paperclip-space-emdash-site
- **Stack:** Astro + Cloudflare (D1 + R2 + Workers)
- **Template:** Blog (posts + pages collections)
- **MCP server:** 26 tools at `/_emdash/api/mcp`

## What Agents Can Do

- Create, edit, publish, schedule, unpublish content via MCP tools
- Upload media via REST API (MCP can't upload)
- Manage categories, tags, navigation menus
- Set SEO metadata (meta title, description) — requires enabling SEO on the collection first
- Monitor 404s and create redirects
- Search content
- Manage site settings

## What Agents Cannot Do

- Change visual design (CSS, templates, themes) — that's code, not data
- Upload media via MCP (REST only)
- Delete content via API on Cloudflare (CSRF blocks DELETE)
- Assign taxonomy terms to posts (endpoint returned 404 in testing)

## Key Findings From Integration Testing

1. Field names are schema-dependent (`content` not `body` for the blog template)
2. Taxonomy names are singular (`category`, `tag`)
3. SEO must be enabled per-collection before the `seo` field works
4. Publish endpoint requires empty JSON body `{}`
5. `codex_local` adapter requires `git init` in workspace
6. Env vars need to be set on each agent individually via UI
7. Some agents need model toggle to validate env vars
8. Manifest goals/projects/issues fields don't import — use CEO onboarding instead

## Next Steps

1. **Set env vars on all 5 agents** and run full team test
2. **Build a setup wizard** on paperclip.space (see `docs/wizard-plan.md`) — interactive onboarding like gstack office-hours
3. **Domain migration** — move from workers.dev URL to final domain
4. **Submit to paperclipai/companies** — PR to the official catalog
5. **Build more company packages** — the `build-agent-company` skill at `~/.paperclip/skills/build-agent-company/` captures the full workflow for future builds

## Architecture

```
paperclip.space (website)
    │
    ├── Setup Wizard (planned)
    │   └── Generates customized company package
    │
    └── emdash instance (CMS)
        └── Managed by Paperclip agent company
            ├── CEO — strategy, board consultation
            ├── Website Manager — editorial calendar, coordination
            ├── Content Writer — drafts in Portable Text
            ├── SEO Specialist — meta tags, redirects, 404s
            └── Distribution Manager — social, newsletters
```
