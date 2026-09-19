# Social Hub Image Processing and Media Specs

## Purpose

The Social Hub should let a user upload an image once and then quietly prepare that image for every destination that uses it.

The user should not need to think about compression, file type, image dimensions, aspect ratio, or platform-specific requirements unless an automatic transformation would materially change the image.

The guiding principle is:

> **The user provides the content once. The Hub handles the technical optimization for each destination.**

---

## Core Workflow

1. User uploads one primary image in the main content workflow.
2. The Hub inspects the image automatically.
3. If the file is unnecessarily large, the Hub resizes and/or compresses it before permanent storage.
4. The optimized master is uploaded to S3.
5. The user chooses where the content will be published, such as:
   - Blog
   - Facebook
   - LinkedIn
   - YouTube
   - Additional social networks added later
6. The Hub determines what image format, dimensions, and aspect ratio each selected destination needs.
7. Destination-specific image variants are generated automatically.
8. If the original image can be adapted safely, the user never needs to intervene.
9. If a destination requires a substantial crop or other meaningful visual change, the Hub flags it for review instead of blindly modifying the image.

---

## Sharp

Use **Sharp** as the primary server-side image processing library.

Sharp can handle tasks such as:

- Resizing
- Compression
- Format conversion
- Metadata inspection
- Reading image width and height
- Calculating aspect ratio
- Generating thumbnails
- Creating destination-specific variants

This keeps image processing inside the existing Node / Next.js application stack without requiring a separate paid image-processing service.

---

## S3 Storage Strategy

S3 remains an essential part of the workflow because posts may be scheduled well in advance. The Hub needs a durable source image available when a scheduled post is eventually published.

However, the system should avoid permanently storing unnecessarily huge original files.

### Recommended approach

Store one **optimized master** rather than blindly saving the full raw upload.

On upload:

- Inspect dimensions and file size.
- If the image is already reasonable, leave it mostly untouched.
- If it is oversized, resize and compress it automatically.
- Preserve enough quality and resolution to generate future platform variants.
- Save that optimized master to S3.

This reduces S3 storage and transfer costs while still preserving a high-quality reusable source.

The Hub can then generate destination-specific derivatives from that master.

Temporary derivatives can either:

- Be generated when a post is created and stored until publication, or
- Be generated close to publish time and discarded later.

The exact retention strategy can be tuned as the scheduling system develops.

---

## Do Not Standardize Everything on WebP

WebP is excellent for web delivery, especially blog content, but it should not be treated as the universal Social Hub source format.

Different platforms support different upload formats and requirements.

A better pattern is:

- Keep a high-quality optimized master internally.
- Generate WebP when WebP is appropriate.
- Generate JPEG or PNG when a social destination requires or prefers those formats.

### Example

**Blog**

- Generate WebP automatically for front-end delivery.
- Potentially create multiple responsive sizes later.

**Facebook / LinkedIn / similar social posts**

- Generate an appropriate JPEG or PNG derivative when needed.

**YouTube thumbnail**

- Generate a platform-ready thumbnail image from the master.

The user should never have to manually choose these formats.

---

## Aspect Ratio Philosophy

Do **not** force every uploaded image into one universal aspect ratio.

That would create too many unintended crops and would fail as the Hub expands to platforms with different presentation styles.

Instead, treat aspect ratio as a **destination and content-type rule**.

The uploaded image remains the master composition. The Hub evaluates whether that composition works for each selected output.

---

## Destination-Aware Image Validation

When the user selects a destination, the Hub should immediately evaluate whether the uploaded image works for that use.

For example, when **YouTube** is enabled:

1. Read the uploaded image dimensions.
2. Calculate its aspect ratio.
3. Compare it with the target thumbnail ratio.
4. Classify the result.

Possible states:

### Ready

The image already fits the destination well.

No user action is needed.

### Auto-adjustable

The image is close enough that the Hub can make a small crop or resize without meaningfully changing the composition.

The Hub handles this automatically.

### Review needed

The image would require a substantial crop, important content may be cut off, or the orientation is fundamentally different from the destination.

The Hub should show a preview and allow the user to adjust or replace the image.

---

## YouTube Example

YouTube demonstrates why the Hub should not use one universal aspect ratio.

Regular video thumbnails typically target a landscape **16:9** composition.

Short-form vertical video uses a fundamentally different **9:16** composition.

When YouTube is selected, the Hub should know whether the content being created is:

- Long-form video
- Short-form / vertical video

Then it can validate the master image accordingly.

If the uploaded primary image already works as the YouTube thumbnail, reuse it.

If not, generate a derivative or ask for review.

---

## Other Social Platforms

The same architecture should be used for every network added to the Hub.

Do not hard-code image behavior throughout individual publishing components.

Instead, build a centralized **media specification layer** that describes the image requirements for each destination and content type.

Examples of targets that may eventually be represented include:

- Landscape
- Square
- Portrait
- Vertical / story / short-form
- Link preview image
- Video thumbnail
- Blog hero image

Pinterest is one example of a platform where portrait-oriented imagery is particularly important, so the system should be capable of handling targets such as **2:3** without changing the upload experience for everyone else.

Platform requirements can change over time, so these specifications should be configuration-driven rather than scattered throughout application code.

---

## Suggested Media Spec Model

Conceptually, the Hub could maintain rules similar to:

```ts
const mediaSpecs = {
  blog: {
    hero: {
      preferredFormat: "webp",
      cropPolicy: "avoid",
    },
  },

  youtube: {
    thumbnail: {
      preferredRatio: "16:9",
      preferredFormats: ["jpeg", "png"],
      cropPolicy: "review-if-significant",
    },
    short: {
      preferredRatio: "9:16",
      cropPolicy: "review-if-significant",
    },
  },

  pinterest: {
    post: {
      preferredRatio: "2:3",
      cropPolicy: "review-if-significant",
    },
  },
};
```

This is illustrative rather than the final implementation.

The important part is that media rules live in one central system.

---

## Crop Safety

Automatic resizing is low risk.

Automatic cropping requires more care.

The Hub should avoid aggressive cropping without user review.

A useful rule is:

> **Automate technical changes. Ask before making meaningful editorial changes.**

Examples of technical changes that can usually happen silently:

- Compression
- File conversion
- Metadata cleanup
- Moderate downscaling
- Small safe crops

Examples that should trigger review:

- Large landscape-to-vertical crop
- Large vertical-to-landscape crop
- Cropping that could remove a face, text, product, logo, or important subject
- Any transformation that significantly changes the intended composition

---

## Future Improvement: Smart Cropping

A later version could make cropping more intelligent by detecting important visual regions such as:

- Faces
- Primary subjects
- Text
- Logos
- Salient objects

That could allow the Hub to create better platform variants automatically while preserving the subject of the original image.

This is not required for the first implementation.

---

## Recommended V1 Behavior

For the first version:

- Accept common image uploads without asking users to preprocess them.
- Inspect dimensions, format, orientation, and file size.
- Compress or resize oversized uploads using Sharp.
- Store one optimized master in S3.
- Preserve enough quality to create later derivatives.
- Maintain media requirements in one central configuration.
- Generate destination-specific images automatically.
- Use WebP for blog/web delivery where appropriate.
- Use destination-compatible formats for social networks.
- Check aspect ratio when destinations are selected.
- Reuse the original composition whenever possible.
- Automatically make minor safe adjustments.
- Flag substantial crops for preview/review.
- Support YouTube thumbnail validation as an early concrete example of the system.

---

## Product Principle

This image workflow should feel invisible most of the time.

The Social Hub should not become an image editor that forces users to understand platform specifications.

Instead:

> **Upload once, select destinations, and let the Hub create the technically correct versions automatically.**

The user should only be brought into the process when automation would materially alter their creative intent.
