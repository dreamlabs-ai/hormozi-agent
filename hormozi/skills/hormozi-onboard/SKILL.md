---
name: hormozi-onboard
description: >
  Onboarding wizard. Hits the user with the dream-outcome pitch first, then walks
  them through the 12 business-context questions one at a time. Saves answers to
  ~/hormozi/business.md. Marks onboarded=true in ~/.claude/hormozi-setup.json.
  Invoked only by the master /hormozi skill — users never call this directly.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
---

# Hormozi Onboard — The Dream Pitch + 12 Questions

You run the onboarding wizard. It has TWO parts that always run in sequence: the dream-outcome pitch (excite), then the 12 questions (capture).

## Part 1: The dream-outcome pitch (ALWAYS FIRST)

Show this verbatim before anything else:

```

═══════════════════════════════════════════════
   🚀  WELCOME TO YOUR HORMOZI AGENT
═══════════════════════════════════════════════

   In about 10 minutes, you'll have...

   ✅  Tweets that sound like you — only sharper

   ✅  YouTube hooks that stop the scroll

   ✅  TikTok scripts engineered for the For You page

   ✅  Instagram posts that hit your saves

   ✅  Emails that actually get opened

   All authentic. All on-brand. All Hormozi-grade.

   Click of a button. Forever.

═══════════════════════════════════════════════

   First step: I need to learn your business.

   12 quick questions. Voice them in if you can —

   it'll take 5 minutes if you talk fast.

   Ready? Just say "yes" and we'll start.

```

**Wait for the user to confirm** before going further. If they say no / not ready / skip, end politely: "No worries — run /hormozi any time you're ready."

## Part 2: The 12 questions (sequential, locked)

When they confirm, set up:

1. `mkdir -p ~/hormozi`

2. If `~/hormozi/business.md` doesn't exist, copy `templates/business-md.template` from the plugin's directory. Replace `{{DATE}}` with today's date. Leave `{{Q*_ANSWER}}` placeholders.

Then ask the 12 questions **one at a time**. Wait for each answer. Save each answer to `business.md` immediately (replace the corresponding `{{QN_ANSWER}}`).

After each answer:
- Acknowledge in ONE short sentence — no analysis, no advice
- Show the progress line: `   ▓▓░░░░░░░░░░░░░░░░░░  Question N of 12  •  X%`
- Show the next question

**The 12 questions:**

### Q1 — Avatar

> 💡  **Who exactly is your dream customer?**

> One sentence. Name them, what they do, what stage they're at.

> *Example: "Solo founders running a $0–10K/month info-product, stuck because they've never built a real offer."*

### Q2 — Dream Outcome

> 💡  **What's the BIG result you help them get?**

> Be concrete and measurable. Numbers + timeframes if you can.

### Q3 — Perceived Likelihood

> 💡  **What proof do you have that it works?**

> Testimonials, case studies, your own results, credentials.

### Q4 — Time Delay

> 💡  **How fast do they get the result?**

> "First small win in X. Full transformation in Y."

### Q5 — Effort & Sacrifice

> 💡  **How much work do they have to do?**

> DIY, done-with-you, or done-for-you? Hours per week?

### Q6 — Top 3 Problems

> 💡  **What are the 3 biggest things in their way?**

### Q7 — Current Offer

> 💡  **What are you selling right now?**

> One line. Target + outcome + timeframe.

### Q8 — Price

> 💡  **What does it cost?**

> Price + cadence (monthly / one-time / per-engagement / annual).

### Q9 — Lead Magnet

> 💡  **What's your free entry point?**

> Type "none yet" if you don't have one.

### Q10 — Traffic Source

> 💡  **Where does your audience find you right now?**

> X, YouTube, Instagram, TikTok, podcast, etc. — be specific about which.

### Q11 — Voice — Casual or Formal?

> 💡  **How do you naturally talk to your audience?**

> Casual + raw, polished + premium, somewhere in between?

### Q12 — Forbidden words

> 💡  **Any words, phrases, or topics you NEVER want in your content?**

> Reply "none" if you don't have any.

## On 'skip'

If the user types `skip`, leave the placeholder, move on. At the end, list what was skipped and offer to fill them in next time.

## On completion

1. Update `~/.claude/hormozi-setup.json` — set `onboarded: true`. Save.

2. Show this:

```

═══════════════════════════════════════════════
   🎉  STEP 1 COMPLETE — YOUR VOICE IS LOCKED IN
═══════════════════════════════════════════════

   📁  ~/hormozi/business.md

   ▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░░  33%

═══════════════════════════════════════════════

   Next up: I'm going to pull your past content so

   the agent learns to sound exactly like you —

   tweets, YouTube transcripts, IG, TikTok.

   100% public scrape. No app connections needed.

   Run /hormozi to continue.

```

## Hard rules — read carefully

- **NEVER analyse the answers.** Don't comment on whether their offer is strong, their pricing is right, their churn is bad, their lead magnet should exist. Just record.
- **NEVER suggest improvements.** They didn't ask. The next step is data, not advice.
- **NEVER lecture about Hormozi frameworks during onboard.** The frameworks live inside the writer subagent — the user doesn't need to see them.
- **Acknowledge in one short sentence.** "Locked in." or "Got it." or a tight observation about the answer's content. Never a paragraph.

## What this skill does NOT do

- Doesn't pull data (that's the next skill)
- Doesn't generate content
- Doesn't analyse anything
- Just asks 12 questions and saves answers
