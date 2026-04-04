# EmDash Blog Team

A portable Paperclip agent company for managing a blog powered by [emdash CMS](https://github.com/emdash-cms/emdash) on Cloudflare.

## Agents

| Agent | Role | Reports To | Skills |
|-------|------|-----------|--------|
| Blog Manager | Editorial direction, content calendar, publishing | -- | emdash, content-strategy |
| Content Writer | Drafting, editing, formatting posts | Blog Manager | emdash, copywriting, copy-editing |
| SEO Specialist | Meta tags, slugs, redirects, audits | Blog Manager | emdash, seo-audit, schema-markup, site-architecture |
| Distribution Manager | Social, email, messaging promotion | Blog Manager | emdash, social-content |

## Teams

- **Editorial** — Blog Manager + Content Writer + SEO Specialist
- **Growth** — SEO Specialist + Distribution Manager

## Prerequisites

1. A running emdash instance deployed on Cloudflare
2. A Personal Access Token (PAT) with `content:read`, `content:write`, `media:read`, `media:write` scopes

## Setup

1. Set environment variables in Paperclip secrets store:
   ```
   EMDASH_URL=https://your-emdash-instance.example.com
   EMDASH_API_TOKEN=ec_pat_your_token_here
   ```

2. Import the company:
   ```bash
   npx paperclipai company import --from ~/.paperclip/local-companies/emdash-blog-team
   ```

## Dependencies

This company requires the `emdash` skill from the `emdash-skills` local company. The manifest references it via relative path. Ensure both `emdash-skills/` and `emdash-blog-team/` exist under `~/.paperclip/local-companies/`.

Additionally, agents reference skills from the marketing-skills-studio (`content-strategy`, `copywriting`, `copy-editing`, `seo-audit`, `schema-markup`, `site-architecture`, `social-content`). These must be available in the Paperclip instance.
