---
name: hormozi-tt
description: >
  Generate Hormozi-style TikTok scripts (30-60 second spoken format). Locked sequence:
  load context + TikTok captions + prompt → user picks 1-5 reference TikToks → user
  submits idea → 5 script variants returned. Falls back to tweets if no TikTok data.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Agent
---

# Hormozi TT — TikTok Script Factory

You produce 5 Hormozi-style TikTok script variants. The flow is locked.

## Hard guardrails

Same as `hormozi-x`.

## Pre-flight

1. Read setup.json. If incomplete, tell user to run `/hormozi`. Stop.

2. Look for `~/hormozi/data/tiktok/posts.json`. If missing, fall back to tweets. If both empty, ask user to paste 1-5 reference scripts.

## Step 1 — Reference picker

Show first 15 TikTok captions (or fallback tweets), first 100 chars each.

```

═══════════════════════════════════════════════
   🎵  /hormozi-tt  •  TIKTOK SCRIPT FACTORY
═══════════════════════════════════════════════

   Pick 1–5 past pieces to model after.

   1.  [first 100 chars...]
   ...

   Reply with 1–5 numbers.

═══════════════════════════════════════════════

```

## Step 2 — Idea

Same one-line idea prompt.

## Step 3 — Generate

Inputs:
- `~/hormozi/business.md`
- 1–5 references
- `prompts/tt.md`
- Idea

Call `hormozi-writer` with `platform: "tiktok"`. Returns 5 variants.

## Step 4 — Render

```

═══════════════════════════════════════════════
   🎵  5 SCRIPT VARIANTS — PICK ONE
═══════════════════════════════════════════════

   ▓ A — [angle name]

   [30-60 sec spoken script]

   📝  [one-line note]

   ───────────────────────────────────────────────

   ▓ B ...

═══════════════════════════════════════════════

   Pick A–E, "again", or "tweak X"

```

## Step 5 — Save

`~/hormozi/output/YYYY-MM-DD_tt_<slug>.md`.

## What this skill does NOT do

Same as `hormozi-x`.
