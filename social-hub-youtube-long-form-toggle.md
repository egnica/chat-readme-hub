# Social Hub — YouTube Long-Form Toggle

## Purpose

Add a focused long-form YouTube publishing workflow to the Social Hub. The goal is to let a user upload a video, choose whether it is short-form or long-form, and, for long-form videos, use AI to generate the metadata and chapter structure needed for a strong YouTube upload while keeping everything editable before publishing.

## Core Upload Flow

1. User uploads a video in the YouTube section of the Social Hub.
2. User chooses between:
   - Short-form video
   - Long-form video
3. When **Long-form** is selected, additional YouTube fields appear.
4. The user can fill the fields manually or click an AI analysis button to generate them.
5. The user reviews and edits everything before publishing.

## Long-Form Fields

When the long-form toggle is active, show fields for the standard YouTube upload information, including:

- Title
- Description
- Tags / keywords
- Chapters
- Other relevant YouTube metadata already supported by the upload workflow

The AI-generated content must remain fully editable.

## AI Analysis Button

Add an action such as **Analyze with AI**.

When clicked, the workflow should:

1. Extract or process the video's audio.
2. Generate a timestamp-aware transcript.
3. Analyze the transcript with an LLM.
4. Generate draft values for:
   - Title
   - Description
   - Tags / keywords
   - Chapters
5. Populate those values into the upload form.
6. Allow the user to review, edit, regenerate individual sections, or ignore the generated content.

AI analysis and YouTube publishing should remain separate actions so the analysis step can never accidentally publish a video.

## Chapters Panel

Long-form videos should have a dedicated chapters area rather than hiding chapters inside the description field.

Suggested UI:

- A segmented sidebar or panel labeled **Chapters**
- One row per chapter
- Each row contains:
  - Timestamp
  - Chapter title
- Add chapter
- Remove chapter
- Reorder chapters
- Edit timestamps
- Edit chapter names
- Regenerate chapters with AI

Example:

```text
00:00 Introduction
01:42 Why this matters
04:18 Setting up the workflow
08:05 Common mistakes
12:31 Final thoughts
```

The user should be able to see exactly what will be sent to YouTube.

## YouTube Chapter Behavior

There is not a separate YouTube Chapters API required for this feature. Chapter timestamps can be written into the YouTube video description in YouTube's recognized timestamp format.

The Social Hub should therefore maintain chapters as structured data in the UI and generate the formatted chapter block automatically when building the final YouTube description.

The user should not have to manually duplicate chapter data in both places.

## Transcript

Because the transcript is already required for chapter generation, retain it as part of the video's analysis data.

Possible uses in the initial version:

- Chapter generation
- Title generation
- Description generation
- Tags / keyword generation
- Ability to inspect or copy the transcript

The transcript becomes reusable source material for future AI features without requiring the video to be transcribed again.

## Regeneration Controls

Do not force the user to rerun the entire analysis when only one section needs improvement.

Useful individual actions:

- Regenerate title
- Regenerate description
- Regenerate tags
- Regenerate chapters

This keeps the workflow faster and gives the user more control.

## Processing State

Video analysis may take time, so provide clear status feedback, for example:

```text
Uploading video…
Extracting audio…
Transcribing…
Generating chapters…
Generating YouTube metadata…
Ready for review
```

Errors should be shown at the step where they occur rather than presenting a generic failure message.

## Draft / Review Workflow

Support saving the upload as a draft before publishing.

The expected sequence is:

```text
Upload video
→ Select Long Form
→ Analyze with AI
→ Review metadata
→ Review chapters
→ Make edits
→ Preview final YouTube data
→ Publish / Schedule
```

Nothing should publish automatically as a side effect of AI analysis.

## YouTube Validation

Before publishing, validate the generated data against YouTube requirements where practical.

Examples:

- Required fields are present
- Title and description lengths are valid
- Chapter timestamps are ordered correctly
- First chapter begins at `00:00`
- Timestamp formatting is valid
- Duplicate or invalid timestamps are flagged

## Other Upload Options

The long-form workflow should leave room for normal YouTube publishing controls such as:

- Made for kids / not made for kids
- Visibility
- Scheduling
- Other upload settings already exposed through the YouTube API

These do not need to be part of the AI generation step.

---

# Phase Two — Long Form to Shorts

A future enhancement could reuse the long-form transcript to identify strong moments that could become short-form videos.

This is intentionally **not part of the first implementation**.

## Possible Future Flow

1. Analyze the long-form transcript.
2. Identify strong self-contained moments.
3. Suggest potential Shorts with start and end timestamps.
4. Let the user approve or adjust the selections.
5. Generate clips from the original video.
6. Convert landscape footage to vertical.
7. Add captions if desired.
8. Export or publish the resulting Shorts.

## Vertical Reframing

A basic center crop would often produce weak results because long-form footage is typically composed for landscape video.

A stronger future implementation could use speaker-aware or subject-aware reframing:

- Detect the primary face / speaker / subject.
- Dynamically position the vertical crop around that subject.
- Adjust the crop as the speaker moves.
- Provide a preview so the user can manually nudge or override framing when necessary.

FFmpeg can perform the underlying video cutting, resizing, cropping, caption burning, and rendering. Face or subject detection would determine where the crop should be positioned.

This effectively moves the feature toward an AI-assisted video editor, so it should be treated as a separate phase after the core YouTube publishing workflow is stable.

## Why This Future Feature Matters

The important architectural point is that the first version already creates the most valuable reusable asset: a timestamp-aware transcript.

That same transcript can later power:

- Short-form clip suggestions
- Highlight detection
- Social post ideas
- Captions
- Alternate titles
- Pull quotes
- Video summaries
- Other repurposed content

This means the initial long-form AI workflow should store its transcript and analysis in a way that can be reused instead of treating them as disposable intermediate data.

---

# Initial Scope Summary

### Build now

- Short-form / long-form toggle
- Long-form metadata fields
- AI video analysis
- Timestamp-aware transcription
- AI-generated title
- AI-generated description
- AI-generated tags / keywords
- Dedicated editable chapters panel
- Automatic chapter formatting for the YouTube description
- Individual regeneration controls
- Processing status
- Draft / review state
- YouTube validation
- Publish and scheduling remain separate from AI analysis

### Build later

- AI-generated Short suggestions from long-form content
- Automatic clip extraction
- Landscape-to-vertical conversion
- Speaker-aware reframing
- Automatic captions
- Short-form rendering / publishing workflow

## Product Principle

The first version should stay focused: **AI helps prepare a high-quality long-form YouTube upload, but the user remains in control of every field before anything is published.**
