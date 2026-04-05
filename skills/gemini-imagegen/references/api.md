# Gemini Image Generation API Reference

## Model

`gemini-3.1-flash-image-preview` — fast, cost-effective image generation with good text rendering.

## Endpoint

```
POST https://generativelanguage.googleapis.com/v1beta/models/gemini-3.1-flash-image-preview:generateContent?key=$GEMINI_API_KEY
```

## Authentication

Pass API key as query parameter: `?key=$GEMINI_API_KEY`

Get a key from https://aistudio.google.com/apikey

## Request Format

```json
{
  "contents": [
    {
      "parts": [
        {
          "text": "Your image generation prompt here"
        }
      ]
    }
  ],
  "generationConfig": {
    "response_modalities": ["IMAGE"]
  }
}
```

**Important:** `response_modalities` must include `"IMAGE"` to get image output. You can also include `"TEXT"` to get a caption alongside the image.

## Response Format

```json
{
  "candidates": [
    {
      "content": {
        "parts": [
          {
            "inlineData": {
              "mimeType": "image/png",
              "data": "base64_encoded_image_data..."
            }
          }
        ]
      }
    }
  ]
}
```

The image is returned as **base64-encoded data** in `candidates[0].content.parts[].inlineData.data`. The MIME type is typically `image/png`.

## Decoding the Image

```bash
# Extract base64 from JSON response and decode to file
curl -s -X POST \
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-3.1-flash-image-preview:generateContent?key=$GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [{"parts": [{"text": "YOUR PROMPT"}]}],
    "generationConfig": {"response_modalities": ["IMAGE"]}
  }' | python3 -c "
import sys, json, base64
r = json.load(sys.stdin)
for part in r['candidates'][0]['content']['parts']:
    if 'inlineData' in part:
        with open('/tmp/generated.png', 'wb') as f:
            f.write(base64.b64decode(part['inlineData']['data']))
        print('Saved to /tmp/generated.png')
        print('MIME:', part['inlineData']['mimeType'])
"
```

## Supported Aspect Ratios

| Ratio | Use Case |
|-------|----------|
| 1:1 | Social media profile, Instagram |
| 16:9 | Blog featured image, YouTube thumbnail |
| 4:3 | Standard landscape |
| 3:2 | Photography style |
| 9:16 | Stories, vertical mobile |
| 21:9 | Ultra-wide banner |

Specify in the prompt: "...in 16:9 landscape aspect ratio"

## Supported Resolutions

| Resolution | Approx Price |
|-----------|-------------|
| 512px (0.5K) | $0.045 |
| 1024px (1K) | $0.067 |
| 2048px (2K) | $0.101 |
| 4096px (4K) | $0.151 |

Default is 1024px. For blog featured images, 1K or 2K is sufficient.

## Rate Limits

- Free tier: ~60 RPM, 500 requests/day
- Paid tier: ~1,000 RPM

## Image Editing (Multi-turn)

You can edit an existing image by sending it back with a modification prompt. This requires the Files API or base64 input image — see `patterns.md` for editing workflows.
