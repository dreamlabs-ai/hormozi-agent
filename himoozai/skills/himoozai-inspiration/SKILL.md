---
name: himoozai-inspiration
description: >
  Load 3–5 creators the user wants to make content like. Pulls their public content
  per platform into ~/himoozai/inspiration/<creator-slug>/. Each platform's content
  becomes the STYLE TEMPLATE source — at create-time, the user picks 1–5 of these
  pieces, and the writer infuses the user's voice INTO the inspiration's structure.
  Marks inspiration_loaded=true. Invoked only by the master /himoozai skill.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
---

# Himoozai Inspiration — Style Anchors

You load 3–5 creators the user wants to model their content style after. Each creator's content gets organized BY PLATFORM under `~/himoozai/inspiration/<creator-slug>/<platform>/` so the platform skills can pick from them at create-time.

## Open

```

═══════════════════════════════════════════════
   🎯  STEP 3 — INSPIRATION (STYLE ANCHORS)
═══════════════════════════════════════════════

   Drop 3–5 creators you want to make content like.

   I'll pull their public content from every platform

   you give me and organise it by category.

   At create-time, you pick 1–5 of THEIR pieces as

   the style template. The agent infuses YOUR voice

   into THEIR structure.

   For each creator, drop:

      Name (or paste)

      X handle (optional)

      YouTube channel URL (optional)

      Instagram handle (optional)

      TikTok handle (optional)

   Drop all 3–5 at once or one at a time — I'll work through them.

═══════════════════════════════════════════════

```

## Per creator

For each creator the user names:

1. Slugify the name (kebab-case, lowercase). Make `~/himoozai/inspiration/<slug>/`.

2. For each platform handle/URL they provided, run the public scrape (same methods as `himoozai-backdata`):

   - **X tweets** → `~/himoozai/inspiration/<slug>/x/tweets.json`
   - **YouTube transcripts** → `~/himoozai/inspiration/<slug>/youtube/<videoId>.txt` (clean from VTT)
   - **Instagram** → `~/himoozai/inspiration/<slug>/instagram/posts.json`
   - **TikTok** → `~/himoozai/inspiration/<slug>/tiktok/posts.json`

3. Each platform pulled independently. If one fails, log and continue.

4. After loading, show:

   ```
   🎉  Loaded <Name>:  ✅ X (47 tweets)  ✅ YT (10 transcripts)  ✴️ IG skipped  ✴️ TT skipped
   ```

## Show running progress

After each creator:

```
═══════════════════════════════════════════════
   🎯  INSPIRATION LOADED SO FAR
═══════════════════════════════════════════════

   1.  Alex Hormozi      ✅ X / ✅ YT
   2.  Codie Sanchez     ✅ X / ✅ YT
   3.  Biaheza           ✴️ X skipped / ✅ YT
   ...

═══════════════════════════════════════════════
```

## On completion

When the user says they're done (or has loaded at least 1 creator with content):

1. Update `~/.claude/himoozai-setup.json` — `inspiration_loaded: true`. Save.

2. Show:

```

═══════════════════════════════════════════════
   ✅  STEP 3 COMPLETE — STYLE ANCHORS LOCKED
═══════════════════════════════════════════════

   📁  ~/himoozai/inspiration/

   ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░  75%

═══════════════════════════════════════════════

   You're ready. Run /himoozai to see the platform menu.

   Each platform skill pulls from these creators when

   you pick reference pieces at create-time.

```

## Hard rules

- **NEVER analyse or judge the creators** — just absorb and organize
- **NEVER suggest different creators** — the user picks
- **NEVER block** — if a platform fails for a creator, move on

## What this skill does NOT do

- Doesn't pick which pieces to use (that's per-creation, in the platform skills)
- Doesn't generate content
- Doesn't analyse creator quality / engagement
