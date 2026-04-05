---
name: gemini-imagegen
description: Generate images using the Gemini API (gemini-3.1-flash-image-preview). Use when asked to create featured images, blog graphics, social media visuals, illustrations, promotional images, or any image generation task. Also use when the task mentions "generate image," "create graphic," "featured image," "social graphic," "illustration," "promo image," "banner," or "visual." For uploading generated images to emdash, use the emdash skill's media upload.
metadata:
  version: 1.0.0
---

# Gemini Image Generation

Generate images from text prompts using Google's Gemini API. Returns base64-encoded images that you decode, save, and upload to the target platform.

## Prerequisites

- `GEMINI_API_KEY` env var must be set (get from https://aistudio.google.com/apikey)

Test connectivity:

```bash
curl -s "https://generativelanguage.googleapis.com/v1beta/models?key=$GEMINI_API_KEY" | head -5
```

## Quick Start

Generate an image in one curl command:

```bash
curl -s -X POST \
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-3.1-flash-image-preview:generateContent?key=$GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [{"parts": [{"text": "A modern minimalist blog header illustration about AI automation, clean lines, blue and white palette"}]}],
    "generationConfig": {"response_modalities": ["IMAGE"]}
  }' | python3 -c "
import sys, json, base64
r = json.load(sys.stdin)
for part in r['candidates'][0]['content']['parts']:
    if 'inlineData' in part:
        with open('/tmp/generated.png', 'wb') as f:
            f.write(base64.b64decode(part['inlineData']['data']))
        print('Saved to /tmp/generated.png')
"
```

For detailed API reference, prompt patterns, and gotchas, see the `references/` directory.

## Workflow: Generate → Upload → Reference

1. Generate image with Gemini (this skill)
2. Upload to emdash media library (emdash skill — `POST $EMDASH_URL/_emdash/api/media`)
3. Get `mediaId` from upload response
4. Reference `mediaId` in content or report it to the requesting agent

See `references/patterns.md` for complete workflows.
