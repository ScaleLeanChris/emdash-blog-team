# Image Generation Patterns

## Blog Featured Image

### Step 1: Generate

Craft a prompt based on the post title and content pillars:

```bash
PROMPT="A modern, clean illustration for a blog post titled '${POST_TITLE}'. Style: minimalist digital art, professional, suitable for a tech blog. Aspect ratio 16:9. No text in the image."

curl -s -X POST \
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-3.1-flash-image-preview:generateContent?key=$GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"contents\": [{\"parts\": [{\"text\": \"$PROMPT\"}]}],
    \"generationConfig\": {\"response_modalities\": [\"IMAGE\"]}
  }" | python3 -c "
import sys, json, base64
r = json.load(sys.stdin)
for part in r['candidates'][0]['content']['parts']:
    if 'inlineData' in part:
        with open('/tmp/featured.png', 'wb') as f:
            f.write(base64.b64decode(part['inlineData']['data']))
        print('Saved: /tmp/featured.png')
"
```

### Step 2: Upload to emdash

```bash
RESPONSE=$(curl -s -X POST \
  -H "Authorization: Bearer $EMDASH_API_TOKEN" \
  -F "file=@/tmp/featured.png" \
  "$EMDASH_URL/_emdash/api/media")

MEDIA_ID=$(echo "$RESPONSE" | python3 -c "import sys,json; print(json.load(sys.stdin)['data']['item']['id'])")
echo "Media ID: $MEDIA_ID"
```

### Step 3: Update alt text

```bash
curl -s -X PUT \
  -H "Authorization: Bearer $EMDASH_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{\"alt\": \"Illustration for: ${POST_TITLE}\"}" \
  "$EMDASH_URL/_emdash/api/media/$MEDIA_ID"
```

### Step 4: Report to requesting agent

Comment on the Paperclip issue with the mediaId so the Content Writer can embed it:

```
mediaId: {MEDIA_ID}
Image uploaded to emdash media library with alt text.
Content Writer: use this mediaId in the post's featured_image field.
```

## Social Media Graphics

### Twitter/X Card (1200x675)

```
PROMPT="Social media card for blog post '${POST_TITLE}'. Clean design, bold visual, no text overlay. Aspect ratio 16:9, optimized for Twitter card display."
```

### LinkedIn Post (1200x627)

```
PROMPT="Professional LinkedIn post graphic for '${POST_TITLE}'. Corporate-friendly, clean typography area at bottom. Aspect ratio close to 2:1."
```

### Instagram Square (1080x1080)

```
PROMPT="Instagram post graphic for '${POST_TITLE}'. Eye-catching, vibrant, square format 1:1. Modern design aesthetic."
```

## Prompt Engineering Tips

### Do
- Be specific about style: "minimalist digital art", "flat vector illustration", "photorealistic"
- Specify aspect ratio in the prompt
- Mention color palette if the site has brand colors
- Say "no text in the image" unless you specifically want text rendered
- Reference the post topic for contextual imagery

### Don't
- Use vague prompts like "make a nice image"
- Request text-heavy images (text rendering is imperfect)
- Assume the model knows your brand — describe the style every time
- Generate images with identifiable real people

## Prompt Template

```
A {style} illustration for a {content_type} about {topic}.
{Color/mood guidance}.
Aspect ratio {ratio}.
{Additional constraints: "no text", "professional", "minimalist"}.
```

Example:
```
A clean, minimalist digital illustration for a blog post about automated content publishing.
Blue and white color palette with subtle gradients.
Aspect ratio 16:9.
No text in the image. Professional, suitable for a tech company blog.
```
