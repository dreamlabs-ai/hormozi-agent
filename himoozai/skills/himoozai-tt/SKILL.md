---
name: himoozai-tt
description: >
  Generate 5 Hormozi-style TikTok scripts (15-60 sec) using the real Short-Form
  Content Style Extractor prompt. Locked sequence: pick 1-5 inspiration short-form
  pieces → submit idea → 5 variants.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Agent
---

# Himoozai TT — TikTok Script Factory

You produce 5 Hormozi-grade TikTok script variants using the real Short-Form Content Style Extractor prompt.

## Hard guardrails

Same as `himoozai-x`.

## Pre-flight

1. Setup must be complete. If not, tell user to run `/himoozai`.

2. Find inspiration TikTok content: `~/himoozai/inspiration/*/tiktok/posts.json`. If none, fall back to `~/himoozai/inspiration/*/x/tweets.json` (short-form voice). If both empty, ask user to paste 1–5 reference scripts.

## Step 1 — Inspiration picker

Build a numbered list of 1-line previews (first 80 chars).

```

═══════════════════════════════════════════════
   🎵  /himoozai-tt  •  TIKTOK SCRIPT FACTORY
═══════════════════════════════════════════════

   Pick 1–5 inspiration pieces.

   1.  [hormozi]  ...
   ...

═══════════════════════════════════════════════

```

## Step 2 — Idea

Standard one-line prompt.

## Step 3 — Generate

Inputs:
- `~/himoozai/business.md`
- User voice sample (tweets if no TikTok data)
- 1–5 inspiration picks
- `prompts/tt.md`
- Idea

Call `himoozai-writer` with `platform: "tiktok"`. Instruct: *"Follow the Short-Form Content Style Extractor prompt. Run Steps 1+2 internally on inspiration_picks. Step 3: produce 5 distinct script variants — different hook styles. Each is a 15–60 sec spoken script with HOOK / SETUP / PAYOFF / CORE VALUE / CLOSING BEAT structure. Return JSON."*

5 variants in JSON.

## Step 4 — Render

```

═══════════════════════════════════════════════
   🎵  5 SCRIPT VARIANTS — PICK ONE
═══════════════════════════════════════════════

   ▓ A — [hook style]

   [30-60 sec spoken script]

   📝  [note]

   ─────────────────────────────────────────────

   ▓ B / C / D / E ...

═══════════════════════════════════════════════

   Pick A–E, or "again", or "tweak X"

```

## Step 5 — Save

`~/himoozai/output/YYYY-MM-DD_tt_<slug>.md`. End session.

## What this skill does NOT do

Same as `himoozai-x`.
