# Social Hub AI Metadata Service Overview

## Purpose

This document captures the high-level discussion around eventually giving the Gignovate social media hub its own AI capabilities while keeping the system flexible, affordable, and less dependent on any single AI provider.

The long-term idea is not necessarily to train a large language model from scratch. Instead, the goal would be to build an independent AI stack that can use an open-source model, run on hardware we control, and connect directly to applications such as the social media hub.

## Long-Term AI Direction

A future self-hosted system could include:

- An upgradable PC or server with a strong GPU and substantial VRAM.
- A local model runner such as `llama.cpp` or Ollama.
- A permissively licensed open-source model such as a Qwen or Mistral-family model, depending on what is strongest and most appropriate at the time.
- A local API layer that applications can call.
- A memory/retrieval layer that can access project information, files, code, notes, or other useful context without requiring the base model to be retrained.
- A web interface or application layer hosted separately from the AI hardware.

The important distinction is that the product would be our own system even if an open-source model is used as the underlying engine. The interface, workflows, APIs, memory, data, and applications would remain under our control.

## Training vs. Retrieval

Training an LLM from scratch would be prohibitively expensive and is not the current goal.

Instead, most personalization could come from retrieval. The model could be given access to relevant project files, notes, code, documentation, transcripts, and other information when responding.

Fine-tuning could potentially be explored later for narrow use cases, but it is not required to build a useful private AI system.

## Hardware Concept

A future dedicated AI PC should be designed around GPU memory and upgradeability rather than simply buying the fastest CPU available.

A strong target system might eventually include:

- A high-end NVIDIA GPU with roughly 32 GB of VRAM.
- 64 GB or more of system RAM.
- 2–4 TB of fast NVMe storage.
- A motherboard, case, cooling system, and power supply selected with future GPU expansion in mind.

A machine at the high end of that range can currently become expensive, so there is no reason to make that purchase before the product actually needs it.

## Public Access to a Self-Hosted AI PC

The AI computer could physically live at home or in another private location without the product being thought of as a "home" application.

A static residential IP would not be required. A service such as Cloudflare Tunnel could provide a secure connection between the public application and the private AI computer.

Conceptually:

`User -> Web App -> Secure API Connection -> AI PC -> Model -> Response`

The user would only see a normal web application. They would not need direct access to the computer or a screen-sharing session.

## Amplify / Next.js Architecture

AWS Amplify could continue to host the public-facing application while the AI model runs elsewhere.

A possible architecture:

1. A Next.js application is hosted through Amplify.
2. The user performs an AI-assisted action in the application.
3. The application sends a secure API request to the AI service.
4. The AI service runs the appropriate model or processing workflow.
5. Structured results are returned to the Next.js application.
6. The application places those results into editable fields for the user.

This separation is valuable because the front-end application does not need to care which AI model is underneath it.

## Social Media Hub: YouTube Use Case

The social media hub could be the first major application powered by this architecture.

For example, when a user uploads or prepares a YouTube video, an **AI Generate** button could trigger a workflow that:

1. Retrieves the uploaded video, potentially from S3.
2. Produces a transcript using speech-to-text processing.
3. Sends the transcript and relevant context to an LLM.
4. Generates structured YouTube metadata such as:
   - Video title
   - Description
   - Chapters / timestamps
   - Tags or keywords
   - Other metadata needed by the YouTube workflow
5. Returns the results as structured JSON.
6. Automatically fills the corresponding fields in the social hub.
7. Allows the user to review and edit everything before publishing.

This creates a much more closed-loop workflow inside the Gignovate platform.

## Start With an API, Not Expensive Hardware

There is no need to buy a $6,000+ AI computer in order to begin building these features.

The practical first version is to use a commercial AI API on a usage-based basis. The ChatGPT consumer subscription itself cannot be embedded into an application, but OpenAI and other providers offer APIs that applications can call directly and pay for based on usage.

For an early product with relatively light usage, this can be dramatically cheaper than purchasing dedicated hardware.

The architecture should therefore be designed so the AI provider is replaceable.

### Phase 1

`Social Hub -> Internal AI Service -> Commercial AI API`

### Later Phase

`Social Hub -> Same Internal AI Service -> Self-Hosted Open-Source Model`

The application should not need to be rebuilt when that change happens. The underlying AI engine can be swapped while the rest of the product remains intact.

## Product Strategy

The social media hub is beginning to look less like a single-purpose utility and more like a potential Gignovate product platform.

The strongest differentiator may not be having a proprietary LLM. It may be owning the workflow around the AI:

- Upload content once.
- Prepare it for multiple platforms.
- Generate platform-specific metadata.
- Transcribe and analyze video.
- Repurpose long-form content.
- Manage publishing and engagement.
- Eventually connect those workflows to AI infrastructure that Gignovate controls.

The recommendation from this discussion was to build and validate the product before pursuing outside investment. A small number of real users can provide much more useful information about the product than trying to raise money before the workflow has been proven.

## Current Direction

For now:

- Continue building the social media hub.
- Treat AI as a modular service rather than hard-wiring the application to one model provider.
- Use a usage-based AI API when the first AI features are ready.
- Make AI-generated content editable rather than automatically publishing it.
- Keep the architecture ready for a future self-hosted open-source model.
- Delay expensive AI hardware until usage justifies it.

The key principle is:

> Build the application now, keep the AI engine replaceable, and move to self-hosted infrastructure when it becomes technically and financially worthwhile.
