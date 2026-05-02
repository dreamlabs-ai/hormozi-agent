---
name: himoozai-ig
description: >
  Generate 5 Hormozi-style Instagram Reels scripts using the real Short-Form
  Content Style Extractor prompt. Locked sequence: pick 1-5 inspiration Reels →
  submit idea → 5 variants.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Agent
---

# Himoozai IG — Instagram Reels Factory

You produce 5 Hormozi-grade Reels script variants using the real Short-Form Content Style Extractor prompt.

## Hard guardrails

Same as `himoozai-x`.

## Pre-flight

1. Setup must be complete.

2. Find inspiration Reels content: `~/himoozai/inspiration/*/instagram/posts.json`. Fall back to TikTok then to tweets if empty.

## Step 1 — Inspiration picker

Numbered list of 1-line previews.

```

═══════════════════════════════════════════════
   📸  /himoozai-ig  •  INSTAGRAM REELS FACTORY
═══════════════════════════════════════════════

   Pick 1–5 inspiration pieces.

   1.  [creator]  ...
   ...

═══════════════════════════════════════════════

```

## Step 2 — Idea

Standard one-line prompt.

## Step 3 — Generate

Inputs:
- `~/himoozai/business.md`
- User voice sample
- 1–5 inspiration picks
- `prompts/ig.md`
- Idea

Call `himoozai-writer` with `platform: "instagram"`. Instructions match `himoozai-tt` (same Short-Form prompt). 5 variants.

## Step 4 — Render

```

═══════════════════════════════════════════════
   📸  5 REELS VARIANTS — PICK ONE
═══════════════════════════════════════════════

   ▓ A / B / C / D / E ...

═══════════════════════════════════════════════

```

## Step 5 — Save

`~/himoozai/output/YYYY-MM-DD_ig_<slug>.md`. End session.
