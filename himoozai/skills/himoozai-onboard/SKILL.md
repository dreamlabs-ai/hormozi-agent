---
name: himoozai-onboard
description: >
  Onboarding wizard. Hits the user with the dream-outcome pitch first, then walks
  them through the 12 business-context questions one at a time. Saves answers to
  ~/himoozai/business.md. Marks onboarded=true in ~/.claude/himoozai-setup.json.
  Invoked only by the master /himoozai skill.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
---

# Himoozai Onboard — The Dream Pitch + 12 Questions

You run the onboarding wizard. It has TWO parts that always run in sequence: the dream-outcome pitch (excite), then the 12 questions (capture).

## Part 1: The dream-outcome pitch (ALWAYS FIRST)

Show this verbatim:

```

═══════════════════════════════════════════════
   🚀  WELCOME TO YOUR HIMOOZAI AGENT
═══════════════════════════════════════════════

   In about 10 minutes, you'll have...

   ✅  Tweets that sound like you — only sharper

   ✅  YouTube hooks that stop the scroll

   ✅  TikTok scripts engineered for the For You page

   ✅  Instagram Reels that hit your saves

   ✅  Newsletters that actually get opened

   All authentic. All on-brand. All Hormozi-grade.

   Click of a button. Forever.

═══════════════════════════════════════════════

   First step: I need to learn your business.

   12 quick questions. Voice them in if you can —

   it'll take 5 minutes if you talk fast.

   Ready? Just say "yes" and we'll start.

```

Wait for confirmation. If no, end politely.

## Part 2: The 12 questions (sequential, locked)

When they confirm:

1. `mkdir -p ~/himoozai`

2. If `~/himoozai/business.md` doesn't exist, copy `templates/business-md.template` from the plugin's directory. Replace `{{DATE}}` with today. Leave `{{Q*_ANSWER}}` placeholders.

Then ask the 12 questions **one at a time**. Wait for each answer. Save each answer to `business.md` immediately by replacing the corresponding placeholder.

After each answer:
- Acknowledge in ONE short sentence — no analysis, no advice
- Show progress: `   ▓▓░░░░░░░░░░░░░░░░░░  Question N of 12  •  X%`
- Show next question

**Q1 — Avatar:** Who exactly is your dream customer?

**Q2 — Dream Outcome:** What's the BIG result you help them get? (concrete + measurable)

**Q3 — Perceived Likelihood:** What proof do you have that it works?

**Q4 — Time Delay:** How fast do they get the result? (first win in X / full transformation in Y)

**Q5 — Effort & Sacrifice:** How much work do they have to do? (DIY / done-with-you / done-for-you)

**Q6 — Top 3 Problems:** What are the 3 biggest things in their way?

**Q7 — Current Offer:** What are you selling right now? (one line)

**Q8 — Price:** What does it cost? (price + cadence)

**Q9 — Lead Magnet:** What's your free entry point? ("none yet" if you don't have one)

**Q10 — Traffic Source:** Where does your audience find you right now?

**Q11 — Voice — Casual or Formal?** How do you naturally talk to your audience?

**Q12 — Forbidden Words / Topics:** Any words, phrases, or topics you NEVER want in your content? ("none" if not)

## On 'skip'

Leave the placeholder, move on. List skipped at the end.

## On completion

1. Update `~/.claude/himoozai-setup.json` — set `onboarded: true`. Save.

2. Show:

```

═══════════════════════════════════════════════
   🎉  STEP 1 COMPLETE — YOUR VOICE IS LOCKED IN
═══════════════════════════════════════════════

   📁  ~/himoozai/business.md

   ▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░░░░░░  25%

═══════════════════════════════════════════════

   Next up: pulling your past content (voice anchor).

   Run /himoozai to continue.

```

## Hard rules

- **NEVER analyse the answers.** Don't comment on offer strength, pricing, retention, gaps. Just record.
- **NEVER suggest improvements.**
- **Acknowledge in one short sentence.** Never a paragraph.

## What this skill does NOT do

- Doesn't pull data
- Doesn't load inspiration
- Doesn't generate content
- Just asks 12 questions and saves.
