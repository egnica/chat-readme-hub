# I Built a Voice-to-Markdown Workflow with ChatGPT, GitHub, and Next.js

A few days ago, I was driving and had a couple ideas pop into my head that I really wanted to remember. I've been working with [Davis Defense](https://www.davisdefenselawyers.com/video/traffic-violations-minnesota) on content about Minnesota's hands-free driving law, so I've gotten careful about not picking up my phone. But I also knew there was a good chance those ideas would be gone by the time I got where I was going. So, I started talking.


https://latestartbucket.s3.us-east-2.amazonaws.com/mobile-phone/ChatGPT-phone-1789927741074-b64ff7d1.webp


I opened ChatGPT and worked through the ideas out loud using the talk function. That part was easy. What I started thinking about afterward was where those conversations go. I didn't want another chat buried in a list somewhere. I wanted actual notes I could find, scan, edit, and keep.

That became a small project I'm calling **Chat Notes**. The concept is pretty straightforward. I talk through an idea in ChatGPT, save the useful parts as a markdown file in a GitHub repository, and view those files in a simple interface I built, so everything's visible at a glance.

GitHub was a natural fit because I was already using it and already had it connected to chatGPT. Markdown keeps it simple. Just text that's easy to edit. GitHub renders it automatically, and the app converts it for the web.

https://latestartbucket.s3.us-east-2.amazonaws.com/mobile-phone/github-screenshot-1789928101399-d653f0ad.webp

The technical setup is minimal: a Next.js app on AWS Amplify, a GitHub repo where each note is a markdown file, and this ChatGPT project with instructions for turning conversations into structured notes.

There are two connections in play. The dashboard uses GitHub to read and manage the markdown files, and there's a button that links straight to the Chat Notes project in ChatGPT. No custom API work there, just a direct project link.

https://latestartbucket.s3.us-east-2.amazonaws.com/mobile-phone/chat-hub-screenshot-1789928238958-313a821f.webp

And that's how this post got written. I talked it through, saved the useful parts, and cleaned it up inside that same workflow.

It's the same kind of practical, lightweight tooling I build for clients. If you're curious about simple automations or custom workflows, feel free to reach out.
