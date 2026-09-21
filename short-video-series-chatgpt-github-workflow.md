# Short Video Series: ChatGPT + GitHub Workflow

**Date:** September 21, 2026

## Series Concept

Create three fast how-to videos based on the workflow behind the Chat Notes application. Each video should work on its own, but the three videos should also connect as a small series.

The overall theme is practical things discovered while building applications that other people could use in their own daily workflow.

### Series Loop

Each video should end by pointing viewers toward the next related video:

1. **ChatGPT Project Instructions** → points to connecting third-party applications.
2. **Connecting Third-Party Applications** → points to using GitHub as a lightweight CMS.
3. **GitHub as a Lightweight CMS** → points back to the Project Instructions workflow that ties everything together.

This creates an internal loop where someone can discover any one of the videos and naturally be directed toward the rest of the series.

---

## Video 1: Persistent Instructions Inside a ChatGPT Project

### Core Idea

Show that a ChatGPT Project can have its own persistent **Project Instructions**, so the user does not need to explain the same workflow again in every new chat.

### Possible Hook

> Did you know you can give a ChatGPT Project instructions that carry across the chats inside that project?

### Talking Points

- A ChatGPT Project can hold related conversations together.
- Project Instructions establish how ChatGPT should behave within that project.
- Instead of repeatedly explaining a workflow, define it once in the Project Instructions.
- A phrase such as **“wrap this up”** can be defined as the trigger for a repeatable workflow.
- In the Chat Notes example, “wrap this up” means taking the useful substance of the conversation and organizing it into a Markdown note.
- The workflow can go beyond summarizing. If ChatGPT is connected to the appropriate external tool and has permission to act, the instructions can define what should happen next.
- This turns an informal conversation into something reusable instead of letting the useful ideas disappear inside old chats.

### Demo Ideas

- Open the ChatGPT Project.
- Briefly show the Project Instructions.
- Show a brainstorming conversation.
- Use the trigger phrase at the end of the conversation.
- Show the resulting structured Markdown note.

### Things to Emphasize

The interesting part is not simply that ChatGPT can summarize a conversation. The interesting part is that **the workflow itself can persist across new chats inside the Project**.

### Outro / Lead to Video 2

Something along the lines of:

> But creating the note is only half of the workflow. Next, I’ll show how I connect ChatGPT to tools like GitHub so the note can actually go somewhere useful.

---

## Video 2: Connecting Third-Party Applications to ChatGPT

### Core Idea

Show that ChatGPT can connect to other applications and services, allowing a conversation to interact with tools where the user's information or workflow already lives.

### Possible Hook

> If you’re constantly copying things out of ChatGPT and pasting them into another application, there may be a better way.

### Talking Points

- ChatGPT can connect to supported third-party tools and services.
- GitHub is a useful example for development and content workflows.
- Notion or other supported applications can provide similar possibilities depending on the workflow and available connection.
- The connection is what gives ChatGPT access to the outside system; Project Instructions define what ChatGPT should do with that access.
- This can reduce repetitive copy-and-paste work between applications.
- Permissions matter: ChatGPT should only have the access needed for the task.
- Avoid exposing API keys or private credentials directly in prompts when a proper connection or server-side integration is available.

### Real Workflow Example

For Chat Notes:

1. Have a conversation in ChatGPT.
2. Project Instructions define how the conversation should be converted into a Markdown note.
3. GitHub is connected to ChatGPT.
4. After approval, the Markdown file can be saved into the GitHub repository.

### Demo Ideas

- Briefly show where connected tools/plugins are managed.
- Show GitHub as the example connection.
- Jump quickly from the ChatGPT conversation to the resulting file in GitHub.
- Keep the technical setup secondary to the practical payoff.

### Things to Emphasize

The main takeaway is **connecting the tools you already use instead of treating ChatGPT as an isolated window**.

### Outro / Lead to Video 3

Something along the lines of:

> So now these notes are automatically ending up as Markdown files in GitHub. But here’s the part I really like: GitHub can become the content system for the application itself.

---

## Video 3: Using GitHub as a Lightweight CMS

### Core Idea

Show how a GitHub repository full of Markdown files can act as a lightweight content management system for a Next.js application.

### Possible Hook

> You might not need a database for every content-driven application. Sometimes a GitHub repo full of Markdown files is enough.

### Talking Points

- Each note can simply be a Markdown file stored in a GitHub repository.
- Markdown keeps the content portable and readable even outside the application.
- The GitHub API can be used to retrieve those files programmatically.
- A Next.js application can render the Markdown as a clean visual interface.
- GitHub also provides version history, so edits to the content are naturally tracked.
- This can work well for notes, documentation, small content libraries, personal knowledge systems, or lightweight blogs.
- It is not necessarily the right architecture for a huge or highly transactional application, but it can be a very practical solution for smaller content-driven tools.

### Demo Ideas

Show the transformation visually:

**Markdown file in GitHub → GitHub API → Next.js page**

A very fast demo could show:

1. The raw `.md` file in the repository.
2. The application fetching that file.
3. The same content rendered as a polished page in the browser.

The visual before-and-after is probably more compelling than spending much time explaining the API itself.

### Things to Emphasize

Keep this approachable. The headline is not really “here is how the GitHub API works.” The headline is:

> I’m using GitHub as the content system behind this application.

Then briefly explain how it works after viewers see the result.

### Outro / Loop Back to Video 1

Something along the lines of:

> And this is why the ChatGPT Project workflow I showed in the first video is useful: the conversation can become the content that powers this application.

That points viewers back to the first video and completes the three-video loop.

---

## Overall Style

These should feel like **quick discoveries from actually building things**, not formal tutorials. Keep each video focused on one idea, show the payoff early, and only explain enough technical detail to make the idea understandable.

The larger content theme could become:

> Small workflow and application ideas I discover while building things that might be useful in your own day-to-day work.
