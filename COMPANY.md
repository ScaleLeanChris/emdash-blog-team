---
name: EmDash Blog Team
description: Agent team for managing an emdash CMS blog — content creation, SEO optimization, taxonomy organization, and multi-channel distribution. Designed for Cloudflare-deployed emdash instances.
slug: emdash-blog-team
schema: agentcompanies/v1
version: 1.0.0
license: MIT
goals:
  - Maintain a consistent publishing cadence with high-quality, SEO-optimized content
  - Grow organic traffic through strategic topic selection, keyword targeting, and technical SEO
  - Distribute content across social channels, newsletters, and messaging platforms to maximize reach
  - Keep the blog organized with clear taxonomy, internal linking, and navigation
---

EmDash Blog Team is a portable Paperclip company for managing a blog powered by [emdash CMS](https://github.com/emdash-cms/emdash) deployed on Cloudflare.

The team is organized around a lean editorial model:

1. **Blog Manager** owns editorial direction, content calendar, and publishing cadence
2. **Content Writer** drafts, edits, and formats blog posts using Portable Text
3. **SEO Specialist** optimizes content for search, manages redirects, and monitors 404s
4. **Distribution Manager** promotes published content across social, email, and messaging channels

All agents share the `emdash` skill for CMS operations. The Blog Manager coordinates work and delegates to specialists.

## Prerequisites

1. A running [emdash](https://github.com/emdash-cms/emdash) instance (Cloudflare or self-hosted)
2. An emdash Personal Access Token (`ec_pat_*`) with `content:read`, `content:write`, `media:read`, `media:write` scopes at EDITOR role or above
3. After importing this company, configure each agent's environment variables via the Paperclip API — see [README.md](README.md) for detailed setup instructions
