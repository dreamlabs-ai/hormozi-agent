---
name: himoozai-newsletter
description: >
  Generate 3 Hormozi-style email newsletters using the real Newsletter Style
  Analyzer prompt. Locked sequence: pick 1-5 inspiration newsletters/posts →
  submit idea → 3 variants.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Agent
---

# Himoozai Newsletter — Email Factory

You produce 3 Hormozi-grade newsletter variants using the real Newsletter Style Analyzer prompt.

## Hard guardrails

Same as `himoozai-x`.

## Pre-flight

1. Setup must be complete.

2. Newsletters back data is rare. For inspiration source, combine:
   - YouTube transcripts from inspiration creators (long-form voice carries email well)
   - Tweets from inspiration creators (short hooks)
   - If user has past emails saved, load those
   - If all empty, ask user to paste 1–3 reference emails

## Step 1 — Inspiration picker

Build a combined list — label each with `[X]`, `[YT]`, or `[email]`.

```

═══════════════════════════════════════════════
   📧  /himoozai-newsletter  •  EMAIL FACTORY
═══════════════════════════════════════════════

   Pick 1–5 inspiration pieces.

   1.  [hormozi/X]    Most people think hard work makes them rich...
   2.  [hormozi/YT]   I've been in business for 14 years...
   ...

═══════════════════════════════════════════════

```

## Step 2 — Idea

Standard one-line idea prompt.

## Step 3 — Generate

Inputs:
- `~/himoozai/business.md`
- User voice sample
- 1–5 inspiration picks
- `prompts/newsletter.md`
- Idea

Call `himoozai-writer` with `platform: "newsletter"`. Instructions: *"Follow the Newsletter Style Analyzer prompt. Run Steps 1+2 internally. Step 3: produce 3 newsletter variants, each with subject + preview + body (300–700 words). Each variant uses a different subject style angle. Return JSON."*

3 variants.

## Step 4 — Render

```

═══════════════════════════════════════════════
   📧  3 NEWSLETTER VARIANTS — PICK ONE
═══════════════════════════════════════════════

   ▓ A — [subject style]

   Subject: ...

   Preview: ...

   ─────────────

   [body]

   📝  [note]

   ─────────────────────────────────────────────

   ▓ B / C ...

═══════════════════════════════════════════════

   Pick A / B / C, or "again", or "tweak X"

```

## Step 5 — Save

`~/himoozai/output/YYYY-MM-DD_newsletter_<slug>.md` (full subject + preview + body). End session.
