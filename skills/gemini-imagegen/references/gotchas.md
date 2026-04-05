# Image Generation Gotchas

## Base64 Response Size

Gemini returns the full image as base64 in the JSON response. A 1024px PNG can be 1-5MB of base64 text. Make sure your curl response handling can handle large payloads. Use `python3` for base64 decoding — shell `base64` command syntax varies across platforms.

## SynthID Watermarks

All Gemini-generated images include invisible SynthID watermarks for authenticity detection. These are imperceptible to humans but detectable by Google's tools. Do not try to remove them.

## Text in Images

Gemini can render text but it's not always perfect. For blog featured images, prefer **no text** in the image — the post title handles that. If you need text (e.g., social media quote cards), use simple, short text and verify the output.

## Content Safety Filters

Gemini applies safety filters. Prompts requesting violent, explicit, or harmful content will be rejected. Keep prompts professional and brand-appropriate.

## No Persistent Sessions

Each API call is stateless. For multi-turn editing (generate → refine), you need to send the image back as input in the next request. For most blog use cases, single-generation is sufficient.

## Rate Limits on Free Tier

Free tier is 500 requests/day, ~60 RPM. For a blog publishing 3-5 posts/week, this is plenty. But if generating multiple variants per post, you could hit limits. Monitor usage at https://aistudio.google.com/.

## File Cleanup

After uploading to emdash, clean up temp files:

```bash
rm -f /tmp/featured.png /tmp/social-*.png
```

Agents should not leave generated images accumulating in the workspace.

## MIME Types

Gemini returns `image/png` by default. emdash accepts PNG, JPEG, WebP, and other common formats. No conversion needed.

## Upload Requires REST, Not MCP

The emdash MCP tools cannot upload media (MCP only has `media_list`, `media_get`, `media_update`, `media_delete`). Uploads must use the REST API: `POST $EMDASH_URL/_emdash/api/media` with multipart/form-data.

## Alt Text

Always set alt text when uploading to emdash. Use the post title or a brief description of the image content. Don't use "AI generated image" as alt text — describe what the image shows.
