---
name: himoozai-yt
description: >
  Generate 3 Hormozi-style YouTube scripts using the real YouTube Script Style
  Extractor prompt. Locked sequence: pick 1-5 inspiration transcripts → submit
  idea → 3 script variants. User's own transcripts auto-load as voice anchor.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Agent
---

# Himoozai YT — YouTube Script Factory

You produce 3 Hormozi-grade YouTube script variants using the real YouTube Script Style Extractor prompt.

## Hard guardrails

Same as `himoozai-x` — content only, no business analysis.

## Pre-flight

1. Read `~/.claude/himoozai-setup.json`. If setup incomplete, tell user to run `/himoozai`. Stop.

2. Find inspiration transcripts: `ls ~/himoozai/inspiration/*/youtube/*.txt`. If none, tell user: *"No YouTube transcripts in your inspiration. Run /himoozai inspiration to add creators."*

## Step 1 — Show inspiration picker

For each inspiration transcript, show: `[creator-slug] first sentence of transcript`. List up to 20.

```

═══════════════════════════════════════════════
   🎬  /himoozai-yt  •  YOUTUBE SCRIPT FACTORY
═══════════════════════════════════════════════

   Pick 1–5 inspiration transcripts. Their structure

   sets the new script's pacing, hook, and rhythm.

   1.  [hormozi]  I've been in business for 14 years...
   2.  [codie]    How do the rich invest? And can we copy them?...
   ...
   20. [biaheza]  I want to try buying a crashed car...

   Reply with 1–5 numbers.

═══════════════════════════════════════════════

```

Wait. Validate. Load full transcript text for each pick.

## Step 2 — Get the idea

Standard one-line idea prompt.

## Step 3 — Generate

Inputs:
- `~/himoozai/business.md`
- User voice sample: 1–2 transcripts from `~/himoozai/data/youtube/` (full text)
- 1–5 inspiration transcripts (full)
- `prompts/yt.md`
- Idea

Call `himoozai-writer` with `platform: "youtube"`. Instruct: *"Follow the YouTube Script Style Extractor prompt. Steps 1+2 (analyze + template) run internally on inspiration_picks. Step 3: produce 3 script variants — each is a different angle on the idea (e.g., problem-led, story-led, contrarian) but all use the inspiration template. Return JSON."*

JSON:
```json
{
  "platform": "youtube",
  "idea": "...",
  "variants": [
    {"label": "A — angle name", "content": "HOOK:\n...\n\nOUTLINE:\n1. ...\n2. ...\n\nCTA:\n...", "notes": "..."},
    ...
  ]
}
```

3 variants.

## Step 4 — Render

```

═══════════════════════════════════════════════
   🎬  3 SCRIPT VARIANTS — PICK ONE
═══════════════════════════════════════════════

   ▓ A — [angle]

   [full script]

   📝  [note]

   ─────────────────────────────────────────────

   ▓ B — ...

   ▓ C — ...

═══════════════════════════════════════════════

   Pick A / B / C, or "again", or "tweak A"

```

## Step 5 — Save

`~/himoozai/output/YYYY-MM-DD_yt_<slug>.md` plain markdown. End session.

## What this skill does NOT do

Same as `himoozai-x`.
