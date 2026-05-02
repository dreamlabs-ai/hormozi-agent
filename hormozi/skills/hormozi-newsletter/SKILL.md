---
name: hormozi-newsletter
description: >
  Generate Hormozi-style newsletter / broadcast emails (subject + preview + body).
  Locked sequence: load context + reference emails (or tweets/transcripts as voice
  fallback) + prompt → user picks 1-5 references → user submits idea → 3 email
  variants returned. Saves chosen variant to ~/hormozi/output/.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Agent
---

# Hormozi Newsletter — Email Factory

You produce 3 Hormozi-style newsletter email variants. The flow is locked.

## Hard guardrails

Same as `hormozi-x`.

## Pre-flight

1. Read setup.json. If incomplete, tell user to run `/hormozi`. Stop.

2. There's no "newsletter back data" — most users won't have past emails saved. So the references default to a mix of tweets + YouTube transcripts (those carry voice). If both empty, ask user to paste 1-3 past emails to model.

## Step 1 — Reference picker

Build a combined list:

- 10 most recent tweets from `~/hormozi/data/tweets.json` (if present), labeled `[X]`
- 5 YouTube transcripts from `~/hormozi/data/youtube/` (first sentence each), labeled `[YT]`

Show:

```

═══════════════════════════════════════════════
   📧  /hormozi-newsletter  •  EMAIL FACTORY
═══════════════════════════════════════════════

   Pick 1–5 past pieces — they set your email voice.

   1.  [X]   [first 80 chars of tweet 1...]

   2.  [X]   [first 80 chars of tweet 2...]

   ...

   11. [YT]  [first sentence of video 1...]

   ...

   Reply with 1–5 numbers.

═══════════════════════════════════════════════

```

## Step 2 — Idea

```

═══════════════════════════════════════════════
   💡  WHAT'S THIS EMAIL ABOUT?

═══════════════════════════════════════════════

   One line. The angle of the email.

═══════════════════════════════════════════════

```

## Step 3 — Generate

Inputs:
- `~/hormozi/business.md`
- 1–5 references (full text)
- `prompts/newsletter.md`
- Idea

Call `hormozi-writer` with `platform: "newsletter"`. Returns 3 variants.

## Step 4 — Render

```

═══════════════════════════════════════════════
   📧  3 EMAIL VARIANTS — PICK ONE
═══════════════════════════════════════════════

   ▓ A — [angle name]

   Subject: [subject line]

   Preview: [preview text]

   ─────────────────

   [body 300-700 words]

   📝  [one-line note]

   ───────────────────────────────────────────────

   ▓ B ...

   ▓ C ...

═══════════════════════════════════════════════

   Pick A / B / C, "again", or "tweak X"

```

## Step 5 — Save

Save to `~/hormozi/output/YYYY-MM-DD_newsletter_<slug>.md`. Include the full subject, preview, and body in the saved file.

## What this skill does NOT do

Same as `hormozi-x`. Doesn't send the email — just produces the text.
