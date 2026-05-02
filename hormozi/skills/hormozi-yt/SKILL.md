---
name: hormozi-yt
description: >
  Generate Hormozi-style YouTube scripts (hook + outline + CTA). Locked sequence: load
  context + transcripts + prompt → user picks 1-5 reference transcripts → user submits
  idea → 3 script variants returned. Saves chosen variant to ~/hormozi/output/.
  Requires onboard + backdata done first.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Agent
---

# Hormozi YT — YouTube Script Factory

You produce 3 Hormozi-style YouTube script variants. The flow is locked: **load → pick references → idea → variants → save**.

## Hard guardrails

Same as `hormozi-x` — no business analysis, no advice, content-only.

## Pre-flight

1. Read `~/.claude/hormozi-setup.json`. If `onboarded` or `data_pulled` is false, tell the user to run `/hormozi` first. Stop.

2. List `~/hormozi/data/youtube/*.txt`. If empty, tell the user: *"No YouTube transcripts in your back data. Run /hormozi backdata to add some, or paste 1-5 reference transcripts directly."*

## Step 1 — Show reference picker

For each `.txt` file in `~/hormozi/data/youtube/`, read the first 1–2 non-empty lines (that's the video's opening sentence — most representative title-like content). Build a numbered list of up to 15:

```

═══════════════════════════════════════════════
   🎬  /hormozi-yt  •  YOUTUBE SCRIPT FACTORY
═══════════════════════════════════════════════

   Pick 1–5 past transcripts to model after.

   These set the pacing, hook style, and rhythm.

   1.  [first sentence from transcript 1...]

   2.  [first sentence from transcript 2...]

   ...

   Reply with 1–5 numbers (e.g. "2, 7, 11").

═══════════════════════════════════════════════

```

Wait for picks. Validate. Load the FULL transcript text for each picked one (not just the first line) into memory.

## Step 2 — Get the idea

```

═══════════════════════════════════════════════
   💡  WHAT'S THE VIDEO ABOUT?

═══════════════════════════════════════════════

   One line. The core concept of the video.

═══════════════════════════════════════════════

```

Wait for the idea.

## Step 3 — Generate

Read inputs:

- `~/hormozi/business.md` (full)
- The 1–5 picked transcripts (full text — these can be long, that's fine)
- The platform prompt: `prompts/yt.md`
- The idea

Call `hormozi-writer` with `platform: "youtube"`, all of the above. Writer returns 3 variants in JSON.

## Step 4 — Render variants

```

═══════════════════════════════════════════════
   🎬  3 SCRIPT VARIANTS — PICK ONE
═══════════════════════════════════════════════

   ▓ A — [angle name]

   HOOK (15–30 sec):
   [hook text]

   OUTLINE:
   1. [beat name] — [body + B-roll]
   2. [beat name] — [body + B-roll]
   ...

   CTA:
   [cta line]

   📝  [one-line note]

   ───────────────────────────────────────────────

   ▓ B — [angle name]
   ...

   ▓ C — [angle name]
   ...

═══════════════════════════════════════════════

   Pick A / B / C

   Or "again" for fresh angles

   Or "tweak A" to refine that one

```

## Step 5 — On selection

Save to `~/hormozi/output/YYYY-MM-DD_yt_<slug>.md` as plain markdown with the full hook + outline + CTA.

End the session.

## What this skill does NOT do

Same as `hormozi-x` — no analysis, no advice, no posting.
