# Chat Hub Blog Draft Publishing Workflow

## Goal

Add a workflow to the Chat Notes Hub that turns a completed Markdown note into a structured blog draft inside the Egner Content Hub.

The Chat Hub should remain focused on capturing and organizing notes. A dedicated ChatGPT Project will handle the conversion from Markdown into the blog object format, metadata generation, GitHub updates, and eventual publication.

## Proposed Workflow

1. Open a completed `.md` note in the Chat Notes Hub.
2. Click a new button such as **Create Blog Draft** or **Send to Blog**.
3. The button:
   - copies the complete Markdown file,
   - opens the dedicated ChatGPT Project for Egner Content Hub publishing.
4. Paste the Markdown into the new chat.
5. The ChatGPT Project:
   - understands the current Egner Content Hub blog object structure,
   - analyzes the article,
   - converts the Markdown into the required blog object,
   - automatically generates the appropriate metadata,
   - validates the object,
   - shows the completed object for review.
6. After explicit approval, the project adds or updates the blog object in the `egner-content-hub` GitHub repository.
7. New blog objects are always saved with `published: false`.
8. The post only becomes public when Nicholas explicitly instructs the project to publish it.

## Chat Hub Responsibility

The Chat Notes Hub should not need to understand the blog object schema.

Its responsibility is only to provide the handoff:

**Chat Note → Create Blog Draft → Dedicated ChatGPT Project**

This keeps the notes application independent from changes to the blog architecture.

## Dedicated ChatGPT Project

Create a separate ChatGPT Project specifically for managing the Egner Content Hub blog workflow.

The project instructions should document:

- the `egner-content-hub` repository,
- the location of the blog data,
- the exact blog object structure,
- required and optional fields,
- slug conventions,
- date conventions,
- image fields,
- SEO fields,
- category and tag conventions,
- how the website consumes blog objects,
- GitHub workflow rules,
- publishing rules.

When the blog object structure changes, the project instructions can be updated without requiring changes to the Chat Notes Hub.

## Automatic Metadata Generation

The ChatGPT Project should generate the complete metadata set based on the article itself.

This can include, depending on the current blog object schema:

- title,
- SEO/meta title,
- slug,
- excerpt,
- meta description,
- categories,
- tags,
- keywords/search terms,
- created or published date fields,
- hero image alt text when an image exists,
- social/share descriptions,
- structured-data fields,
- any other metadata required by the blog object.

Metadata should be derived from the actual article content and optimized for clarity and relevant search intent without changing the meaning or voice of the article.

Fields that require real external information should not be invented. For example, if an image URL is required but no image has been supplied, the project should flag the missing field instead of fabricating a URL.

## Draft-First Publishing Rule

Every newly created blog object must default to:

```js
published: false
```

Creating the blog object and committing it to GitHub does **not** mean the post is published.

The project must never set:

```js
published: true
```

unless Nicholas explicitly instructs it to publish that specific post.

This creates two distinct actions:

### Create Blog Draft

- Convert Markdown into the blog object.
- Generate metadata.
- Validate the object.
- Show the proposed result.
- Receive explicit approval before modifying GitHub.
- Add the object to `egner-content-hub`.
- Keep `published: false`.

### Publish Blog Post

- Only occurs after an explicit instruction from Nicholas.
- Locate the existing blog object.
- Change `published` from `false` to `true`.
- Show the proposed GitHub change.
- Receive explicit approval before modifying GitHub.
- Commit the update.
- Allow the existing deployment process to make the post public.

## GitHub Safety Rules

Before changing the repository, the project should:

- show the complete proposed blog object,
- identify the target file,
- identify whether this is a new post or an update,
- check for an existing matching slug,
- validate required fields,
- display the current `published` status,
- receive explicit approval before performing any GitHub write action.

After a GitHub update, report:

- repository,
- file changed,
- whether the object was created or updated,
- commit information,
- current draft/published state.

## Duplicate and Update Handling

Before creating a new object, check whether the slug or post already exists.

If it exists, treat the request as an update to the existing blog object rather than creating a duplicate.

The original Markdown note in the Chat Notes Hub should remain independent from the blog object. The note is the source material; the Egner Content Hub object is the publishing representation.

## Desired User Experience

The workflow should ultimately feel like:

**Chat Note → Create Blog Draft → Paste → Review → Save Draft → Publish When Ready**

The goal is to make turning a conversational idea into a structured blog post fast while retaining an explicit human approval step before repository changes and before publication.
