---
name: hormozi-inspiration
description: >
  Pulls public content from creators the user wants to make content like. Stores
  per-creator content under ~/hormozi/inspiration/<slug>/ and writes a summary.md
  per creator describing voice patterns. Marks step4_inspiration true when at least
  one creator is loaded.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
---

# Hormozi Inspiration — Creators You Want to Sound Like

You pull public content from 3–5 creators the user admires so the agent learns their voice patterns. Follow the dreamlabs-style rules.

## Open

```

═══════════════════════════════════════════════
   🎯  INSPIRATION — CREATORS YOU LOOK UP TO
═══════════════════════════════════════════════

   Drop 3–5 creators you want to make content like.

   Just names, URLs, X handles, or YouTube channels.

   I'll pull their public content and learn their patterns.

═══════════════════════════════════════════════

```

## Per creator

For each creator the user names:

1. Slugify the name (e.g., "Alex Hormozi" → `alex-hormozi`).

2. Identify their main public sources:
   - YouTube channel → pull the most recent ~10 transcripts via `yt-dlp` (same flow as hormozi-backdata)
   - X handle → pull the most recent ~50 tweets
   - Public articles / blog posts → if URLs given, fetch with curl

3. Save raw content under `~/hormozi/inspiration/<slug>/`:
   ```
   ~/hormozi/inspiration/alex-hormozi/
   ├── tweets.json
   ├── youtube/
   │   ├── abc123.txt
   │   └── def456.txt
   └── summary.md
   ```

4. Write `summary.md` with voice analysis:

```markdown
# Alex Hormozi — Voice Notes

**Hook style:** Problem-first. Often opens with a contradiction or counterintuitive claim.

**Sentence length:** Short. Often 3–8 words. Builds rhythm.

**Recurring frames:** "Most people [X]. The truth is [Y]." Math-driven framings.

**Common openers:** "Here's the thing.", "Business is simple.", "Let me explain..."

**Vocabulary tells:** "leverage", "compound", "stack", "bottleneck", "free", "leverage"

**Structure pattern:** Hook → cost of inaction → simple framework → example → CTA

**3 example hooks worth stealing:**
1. "[paste actual hook from their content]"
2. "[paste actual hook]"
3. "[paste actual hook]"
```

The summary is what gets loaded by hormozi-idea and hormozi-generate later — it's the high-leverage artifact, not the raw content.

5. Show: `🎉 Loaded Alex Hormozi (12 transcripts, 47 tweets, summary.md written)`

## On completion

After at least one creator is loaded, update `~/.claude/hormozi-setup.json` — set `step4_inspiration: true`.

```

═══════════════════════════════════════════════
   ✅  INSPIRATION LOADED — N creators
═══════════════════════════════════════════════

   📁  ~/hormozi/inspiration/

   Ready for **Step 5: Generate your first idea**?

   Try: /hormozi idea "your topic here"

═══════════════════════════════════════════════

```

## Tone

Curious. After each creator, comment on something specific you noticed about their voice — makes it feel alive.

## What this skill does NOT do

- Doesn't scrape anything that requires login
- Doesn't republish or copy their content
- Just absorbs voice patterns
