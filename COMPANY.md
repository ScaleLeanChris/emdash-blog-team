---
name: EmDash Blog Team
description: Agent team for managing an emdash CMS blog — content creation, SEO optimization, taxonomy organization, and multi-channel distribution. Designed for Cloudflare-deployed emdash instances.
slug: emdash-blog-team
schema: agentcompanies/v1
version: 1.0.0
license: TBD
authors:
  - name: Chris Guill
  - name: Claude Opus 4.6
goals:
  - Maintain a consistent publishing cadence with high-quality, SEO-optimized content
  - Grow organic traffic through strategic topic selection, keyword targeting, and technical SEO
  - Distribute content across social channels, newsletters, and messaging platforms to maximize reach
  - Keep the blog organized with clear taxonomy, internal linking, and navigation
---

EmDash Blog Team is a portable Paperclip company for managing a blog powered by [emdash CMS](https://github.com/emdash-cms/emdash) deployed on Cloudflare.

The team is organized around a lean editorial model:

1. **CEO** sets strategic direction, defines content pillars, and approves consequential decisions
2. **Website Manager** owns the content calendar, publishing cadence, and day-to-day coordination
3. **Researcher** researches topics, finds sources, and produces research briefs via Tavily
4. **Content Writer** drafts posts from research briefs, formats in Portable Text
5. **Graphics Designer** generates featured images and social graphics using Gemini AI
5. **SEO Specialist** optimizes content for search, manages redirects, and monitors 404s
6. **Distribution Manager** promotes published content across social, email, and messaging channels

All agents share the `emdash` skill for CMS operations. The CEO sets direction, the Website Manager coordinates execution, and specialists handle their domains.

## Prerequisites

1. A running [emdash](https://github.com/emdash-cms/emdash) instance (Cloudflare or self-hosted)
2. An emdash Personal Access Token (`ec_pat_*`) with `content:read`, `content:write`, `media:read`, `media:write` scopes at EDITOR role or above
3. After importing this company, configure each agent's environment variables via the Paperclip API — see [README.md](README.md) for detailed setup instructions
