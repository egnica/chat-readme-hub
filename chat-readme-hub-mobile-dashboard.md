# Chat README Hub — Mobile Dashboard

## Overview

This document captures the idea for a lightweight mobile-friendly dashboard for the `chat-readme-hub` GitHub repository.

The repository is intended to act as a simple archive of Markdown files created from useful ChatGPT conversations. The goal is to make those files easy to browse from a phone without creating another system that has to be manually maintained.

## The Problem

The repository itself is the source of truth, but browsing a growing collection of Markdown files directly in GitHub may become visually cumbersome on a phone.

A manually maintained `README` index would solve the navigation problem, but it would create another piece of content that has to be updated every time a Markdown file is added, renamed, or removed.

Using a separate tool such as Notion would introduce similar duplication and syncing concerns.

## Core Goal

Create a very lightweight mobile dashboard that:

- Shows the Markdown files currently in `chat-readme-hub`.
- Updates automatically from GitHub.
- Requires no manually maintained index.
- Keeps GitHub as the single source of truth.
- Is easy to open and browse from a phone.
- Lets the user tap a file to read it.

## Proposed Solution

Build a small read-only web application hosted with AWS Amplify.

The app would connect to the GitHub API and dynamically retrieve the Markdown files in the `chat-readme-hub` repository.

### Basic Flow

1. User opens the dashboard on a phone.
2. The app requests the current repository contents from GitHub.
3. The app filters the results to Markdown files.
4. The files are rendered as a clean mobile-friendly list or card view.
5. Selecting a file opens its contents for reading.

Because the file list comes directly from GitHub, adding a new Markdown file to the repository automatically makes it available in the dashboard.

## Architecture

```text
chat-readme-hub GitHub repository
            ↓
        GitHub API
            ↓
 Lightweight web application
            ↓
       AWS Amplify
            ↓
      Mobile browser
```

GitHub remains the source of truth. The Amplify application is only a presentation layer.

## V1 Scope

Keep the first version intentionally small.

### Repository View

Display:

- File name
- Human-friendly title derived from the file name
- Optional last-modified information if it is useful and inexpensive to retrieve
- Tap target for opening the file

Possible presentation:

```text
Chat README Hub

[ Social Content Hub — Chat Work Workflow ]
[ Social Hub — AI Metadata Service Overview ]
[ Social Hub — Image Processing & Media Specs ]
[ Social Hub — YouTube Long Form Toggle ]
```

### File View

When a file is selected:

- Retrieve the Markdown file from GitHub.
- Render the Markdown as readable HTML.
- Provide an easy way to return to the file list.

### Mobile First

The interface should prioritize phone use:

- Large tap targets
- Minimal navigation
- Fast loading
- Clear typography
- Little or no visual clutter

## Authentication / Repository Access

If `chat-readme-hub` remains public, the first version may be able to use GitHub's public repository endpoints without storing a personal GitHub token in the browser.

If the repository later becomes private, authentication should be handled server-side so credentials are never exposed to the client.

## What This Avoids

The dashboard should not require:

- A manually maintained file index
- Duplicating Markdown into Notion
- A separate content database
- Manual syncing
- Editing metadata every time a file is created

The repository itself already contains the information the dashboard needs.

## Possible Future Enhancements

Only add these if the growing repository creates a real need for them:

- Search
- Sort by newest / oldest
- Categories or inferred groupings
- Folder navigation
- Recently added files
- Favorites or pinned files
- Short previews extracted from the Markdown
- Tags derived from front matter
- GitHub commit date / last-updated date
- Installable PWA behavior for a more app-like phone experience

## Guiding Principle

Do not create another content-management system.

The purpose of this application is simply to make `chat-readme-hub` easier to see and navigate.

**GitHub stores the Markdown. The dashboard renders what is already there.**
