# Onboarding Checklist

Run this on your first heartbeat when no published content exists. Use the Paperclip skill for all coordination (creating goals, projects, issues, assigning work). Use the emdash skill for all CMS operations.

## Step 1: Verify emdash connection

Before anything else, confirm your emdash access works:
- Use `schema_list_collections` to see what collections exist
- Use `schema_get_collection` with `includeFields: true` on each collection to discover field names
- Document the field names (they vary by template — e.g., `content` vs `body`)

If this fails, your `EMDASH_URL` or `EMDASH_API_TOKEN` env vars are not configured. Report this as blocked.

## Step 2: Create company goal and project

Using the Paperclip skill:
- Create a company-level goal: "Launch and operate the emdash website"
- Create a project: "Website Setup" linked to that goal

## Step 3: Ask the board for site direction

**Do not define the editorial mission yourself.** You don't know what this site is for. Create an issue and assign it to the board (human) requesting:

- What is this website about? Who is the target audience?
- What are the primary topics or themes?
- What is the voice and tone? (formal, casual, technical, etc.)
- What is the goal? (lead generation, thought leadership, documentation, community, etc.)
- Any existing brand guidelines or content to reference?

Mark this issue as **critical priority**. Block on it — do not proceed with Steps 4-5 until the board responds with direction.

## Step 4: Define editorial mission from board input

Once the board responds, synthesize their input into:
- 3-5 content pillars (core topics)
- Voice and tone guidelines
- Target publishing cadence
- Success criteria

Document this as a comment on the goal. This becomes the strategic foundation for all downstream work.

## Step 5: Create onboarding tasks for the team

Only after the editorial mission is defined, create and assign these issues (all linked to the Website Setup project):

1. **"Set up taxonomy structure"** → assign to Website Manager
   - Create categories and tags based on content pillars
   - Taxonomy names are singular (`category`, `tag`)
   - Set up primary navigation menu

2. **"Write and publish first post"** → assign to Content Writer
   - Check schema for field names first
   - Write in Portable Text format (not HTML)
   - Submit as draft, then publish after review

3. **"Configure SEO foundation"** → assign to SEO Specialist
   - Enable SEO on the posts collection (requires ADMIN token)
   - Verify sitemap works
   - Check for 404s
   - Set meta title and description on the first post

4. **"Set up content distribution channels"** → assign to Distribution Manager
   - Identify available social/messaging tools
   - Create templates for post-publish promotion

## After onboarding

Once all tasks are done, shift to ongoing operations:
- Website Manager maintains the content calendar and publishing cadence
- Content Writer produces posts on cadence
- SEO Specialist monitors 404s and optimizes each post
- Distribution Manager promotes each published post
- You review performance and adjust strategy as needed
