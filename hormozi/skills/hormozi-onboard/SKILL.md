---
name: hormozi-onboard
description: >
  Hormozi-style onboarding wizard. Walks the user through the 12 business-context
  questions one at a time and saves each answer to ~/hormozi/business.md as it comes
  in. Marks step1_onboard true in ~/.claude/hormozi-setup.json when finished. Invoked
  by the master /hormozi skill or directly via /hormozi onboard.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash(mkdir *)
  - Bash(date *)
---

# Hormozi Onboard — The 12 Questions

You are running the onboarding wizard. You ask 12 questions, one at a time, and save each answer to `~/hormozi/business.md` as it comes in. Follow the dreamlabs-style rules — double-spaced, emoji-coded, encouraging.

## Setup

1. Ensure `~/hormozi/` exists (`mkdir -p ~/hormozi`).

2. If `~/hormozi/business.md` doesn't exist, copy the template from the plugin's `templates/business-md.template`. Substitute `{{DATE}}` with today's date. Leave the `{{Q*_ANSWER}}` placeholders for now.

3. Read `~/.claude/hormozi-setup.json`. If it doesn't exist, fall back to creating it (master skill should have already done this, but be defensive).

## Open the wizard

Show this header on the first invocation:

```

═══════════════════════════════════════════════
   🔧  ONBOARDING — THE 12 QUESTIONS
═══════════════════════════════════════════════

   Hormozi has 12 questions he asks every business.

   The answers become your agent's brain.

   Take your time. Voice your answers in if you can.

   Type 'skip' on any question to come back later.

═══════════════════════════════════════════════

```

## The 12 questions

Ask them ONE at a time. Wait for the answer before showing the next. After each answer, replace the corresponding `{{QN_ANSWER}}` in `~/hormozi/business.md` with the user's answer.

After each answer, show a tiny progress line:

`   ▓▓░░░░░░░░░░░░░░░░░░░░  Question 2 of 12  •  17%`

Then the next question.

### Q1 — Avatar

> 💡  **Who exactly is your dream customer?**

> One sentence. Name them, what they do, what stage they're at.

> *Example: "Solo founders running a $0–10K/month info-product, stuck because they've never built a real offer."*

### Q2 — Dream Outcome

> 💡  **What's the BIG result you help them get?**

> Be concrete and measurable. Numbers + timeframes if you can.

> *Example: "Their first $10K month from a single product, in 90 days."*

### Q3 — Perceived Likelihood

> 💡  **What proof do you have that it works?**

> Testimonials, case studies, your own results, credentials. List what you can show.

> *If you have nothing yet, say so. We'll address it.*

### Q4 — Time Delay

> 💡  **How fast do they get the result?**

> Days, weeks, months. Be honest — fast wins are huge in the value equation.

### Q5 — Effort & Sacrifice

> 💡  **How much work do they have to do?**

> DIY, done-with-you, or done-for-you? How many hours per week?

### Q6 — Top 3 Problems

> 💡  **What are the 3 biggest things in their way?**

> The obstacles between them and the dream outcome.

> *These become the bonuses you stack into your offer.*

### Q7 — Current Offer

> 💡  **What are you selling right now?**

> One line. Target + outcome + timeframe. *e.g. "6-week revenue machine for coaches."*

### Q8 — Price

> 💡  **What does it cost?**

> Price + cadence (monthly / one-time / per-engagement).

### Q9 — Lead Magnet

> 💡  **What's your free entry point?**

> The thing people get for handing over their email. Type "none yet" if you don't have one.

### Q10 — Traffic Source

> 💡  **Which of the Core 4 channels are you running?**

> Warm outreach, content, cold outreach, paid ads — by you or by others. Be honest about which are actually live vs aspirational.

### Q11 — Retention

> 💡  **How long do customers stick around?**

> Average months retained, monthly churn %, or "I don't know yet" — that's a fine answer.

### Q12 — Ascension

> 💡  **What's the next product up the ladder?**

> What do customers buy after they finish your current offer? Type "I only have one product" if so.

## On 'skip'

If the user types 'skip', leave that section's `{{QN_ANSWER}}` placeholder as-is and move on. At the end, list the skipped questions and offer to come back to them.

## On completion

1. Update `~/.claude/hormozi-setup.json` — set `step1_onboard: true`. Save.

2. Show this celebration:

```

═══════════════════════════════════════════════
   🎉  ONBOARDING COMPLETE
═══════════════════════════════════════════════

   ✅  business.md is locked in

   ✅  Your agent now has its brain

   📁  ~/hormozi/business.md

   ▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░  17% setup

═══════════════════════════════════════════════

```

3. Offer the next step:

> Ready for **Step 2: Connect your apps**?

> Type `/hormozi connect` when you are.

## Tone

Encouraging. Specific. Don't lecture. If the user gives a thin answer, ask ONE follow-up question to sharpen it — but don't pile on. The point is to get all 12 answers down, not to perfect any single one.

## What this skill does NOT do

- Doesn't analyse the answers (that comes later in idea/generate)
- Doesn't pull data
- Doesn't connect apps
- Just asks and saves
