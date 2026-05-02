---
name: hormozi-ig
description: >
  Generate Hormozi-style Instagram captions. Locked sequence: load context + IG posts
  + prompt → user picks 1-5 reference captions → user submits idea → 5 caption variants
  returned. Falls back to tweets if no IG data exists. Saves chosen variant to
  ~/hormozi/output/.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Agent
---

# Hormozi IG — Instagram Caption Factory

You produce 5 Hormozi-style IG caption variants. The flow is locked: **load → pick references → idea → variants → save**.

## Hard guardrails

Same as `hormozi-x` — no business analysis, no advice.

## Pre-flight

1. Read `~/.claude/hormozi-setup.json`. If setup incomplete, tell user to run `/hormozi`. Stop.

2. Look for `~/hormozi/data/instagram/posts.json`. If missing, fall back to `~/hormozi/data/tweets.json` (tweets work as IG voice references). If both empty, ask the user to paste 1-5 captions to model.

## Step 1 — Show reference picker

Show first 15 IG posts (or fallback tweets). For IG, show first 100 chars of caption.

```

═══════════════════════════════════════════════
   📸  /hormozi-ig  •  INSTAGRAM CAPTION FACTORY
═══════════════════════════════════════════════

   Pick 1–5 past captions to model after.

   1.  [first 100 chars of caption 1...]

   2.  ...

   Reply with 1–5 numbers (e.g. "2, 7, 11").

═══════════════════════════════════════════════

```

Wait for picks. Validate. Load full captions into memory.

## Step 2 — Get the idea

Same as other skills — one-line idea prompt.

## Step 3 — Generate

Inputs:
- `~/hormozi/business.md`
- 1–5 reference captions
- `prompts/ig.md`
- Idea

Call `hormozi-writer` with `platform: "instagram"`. Writer returns 5 variants.

## Step 4 — Render variants

```

═══════════════════════════════════════════════
   📸  5 CAPTION VARIANTS — PICK ONE
═══════════════════════════════════════════════

   ▓ A — [angle name]

   [caption text]

   📝  [one-line note]

   ───────────────────────────────────────────────

   ▓ B — [angle name]
   ...

═══════════════════════════════════════════════

   Pick A–E, or "again", or "tweak X"

```

## Step 5 — Save

Save to `~/hormozi/output/YYYY-MM-DD_ig_<slug>.md`.

## What this skill does NOT do

Same as `hormozi-x`.
