# Happy Healthy Wealthy — Marketing Funnel & Technical Architecture

## Goal

Build a focused first-version marketing funnel for Happy Healthy Wealthy (H2W) that turns podcast listeners and other visitors into identifiable leads, nurtures those leads, and ultimately invites qualified people to schedule a complimentary H2W Fit Call.

## Core Funnel

### 1. Podcast as the starting point

The podcast is the initial content engine and a primary way people can discover H2W.

Ideally, episodes are produced as both **video and audio** so the long-form content can also be used for social clips.

Each podcast episode can have its own slug page on the H2W Next.js website, similar to the video-page structure used for Davis Defense.

Potential episode page elements:

- Embedded video and/or audio
- Episode title and description
- Chapters
- Transcript
- Structured/JSON-LD data
- Clear CTA to the H2W recovery assessment

The podcast itself can also include a natural verbal CTA directing listeners to the assessment.

## 2. H2W Recovery Check-In / Quiz

Create a short, approximately two-minute assessment directly on the H2W website.

The visitor completes the quiz first and enters their name/email at the end to receive their results.

Flow:

Podcast / Content → Recovery Quiz → Email Capture → Results

The result should:

- Display immediately on the website
- Be emailed to the person
- Give them something genuinely useful
- Introduce the option of scheduling a complimentary H2W Fit Call

### Recovery Categories

Ask Moe to identify approximately 4–6 primary recovery situations H2W is designed to help.

The quiz can store the person's recovery category so future communication can be lightly personalized.

Avoid building completely separate funnels for every category initially. Start with one core nurture sequence and personalize portions based on the lead's recovery type.

## 3. Email Nurture

Use **Resend** for transactional and nurture emails.

### Initial Email

Immediately after completing the assessment:

- Send quiz results
- Provide useful context
- Introduce the H2W Fit Call as an optional next step

### Follow-Up Sequence

If the person has not booked a Fit Call, send approximately **3–4 additional emails** over the following days/weeks.

Emails should primarily be helpful and educational, with a gentle CTA to schedule a Fit Call.

If someone does not book after the sequence, they can move onto Moe's regular email/newsletter list.

### Tracking

Use Resend webhooks to track relevant email activity such as:

- Sent
- Delivered
- Opened
- Clicked
- Bounced
- Complained

A click on the Fit Call CTA should be tracked, but a click should **not** be treated as a confirmed booking. The booking platform needs to provide confirmation that an appointment was actually scheduled.

## 4. Lead Database

Use **MongoDB** to maintain the state of each lead and the funnel.

Potential data includes:

- Name
- Email
- Quiz answers
- Quiz result
- Recovery category
- Lead source
- Podcast episode / campaign source
- Date quiz completed
- Emails sent
- Email engagement
- Fit Call CTA clicked
- Fit Call booked
- Current funnel stage
- Next scheduled action

MongoDB becomes the source of truth for where each person currently sits in the funnel.

## 5. Scheduled Email Automation

Use AWS infrastructure for timed nurture emails.

Initial architecture:

**MongoDB → AWS EventBridge Scheduler → AWS Lambda → Resend**

MongoDB maintains lead/funnel state.

EventBridge Scheduler handles timing.

Lambda checks whether the lead is still eligible for the next email — for example, confirming they have not already booked a Fit Call — and then triggers the appropriate Resend email.

## 6. Booking

**Booking system: TBD**

Before integrating this portion, confirm whether Moe already uses a scheduling/booking platform.

The eventual integration should allow the system to know when someone has actually booked a Fit Call so unnecessary nurture emails can stop or change.

Once booked, the lead can move into a small booked-call flow such as:

- Confirmation
- Reminder
- Post-call status

## 7. Quo / SMS

Quo is **not required for V1**.

It could later be incorporated for:

- SMS appointment reminders
- Follow-up texts
- Other appropriate client communication

Keep this outside the initial build until the core funnel is working.

## 8. H2W Admin Dashboard

Build a small private admin dashboard rather than trying to create a full CRM immediately.

Host the Next.js application/dashboard through **AWS Amplify**.

Initial dashboard should make it easy to see:

- Leads
- Recovery category
- Quiz status/results
- Email status
- Engagement
- Fit Call status
- Current funnel stage
- Next action

The goal is visibility into what the funnel is actually doing without overbuilding V1.

## V1 Technical Architecture

- **Website / Assessment:** Next.js
- **Hosting:** AWS Amplify
- **Database:** MongoDB
- **Email:** Resend
- **Email Event Tracking:** Resend Webhooks
- **Scheduled Automation:** AWS EventBridge Scheduler
- **Serverless Processing:** AWS Lambda
- **Booking:** TBD
- **SMS:** Quo — possible later addition
- **Admin:** Small private H2W dashboard

## Open Questions for Moe

1. Will the podcast be audio-only or video + audio?
2. When is the next podcast recording/shoot planned?
3. What are the 4–6 primary recovery situations H2W most wants to serve?
4. Does Moe currently use a booking/scheduling platform for H2W Fit Calls?
5. What should happen operationally after a Fit Call is booked?
6. What email/newsletter system or list, if any, already exists?

## Current Working Funnel

**Podcast / Social Content**

↓

**H2W Podcast Episode Page**

↓

**2-Minute H2W Recovery Check-In**

↓

**Quiz Result + Email Capture**

↓

**Immediate Results Email**

↓

**3–4 Email Nurture Sequence**

↓

**Complimentary H2W Fit Call**

↓

**Booked Call Follow-Up**

If the person does not book, they can transition into H2W's ongoing email/newsletter audience.

---

## Future Layers — Not Required for V1

Once the core funnel is operating and measurable, additional acquisition and communication channels can feed into the same system:

- Social media campaigns
- Podcast clips
- YouTube
- Organic search
- Paid advertising
- Retargeting
- SMS
- Additional lead magnets
- More advanced segmentation

The goal is to establish one understandable, measurable customer path first and expand only after that path is working.
