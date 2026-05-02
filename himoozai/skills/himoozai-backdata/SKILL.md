---
name: himoozai-backdata
description: >
  Pulls the user's past content from public sources (X, YouTube, Instagram, TikTok)
  into ~/himoozai/data/. No app connections — pure public scrape. This data is the
  user's VOICE anchor (auto-loaded on every generation). Each platform pulled
  independently; if one fails, others still complete. Marks data_pulled=true.
  Invoked only by the master /himoozai skill.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
---

# Himoozai Backdata — User's Voice Anchor

You pull the user's public content into `~/himoozai/data/`. This becomes their VOICE anchor — the agent samples from this on every future generation to keep their authentic sentence rhythm, vocabulary, and openers.

## The single question

```

═══════════════════════════════════════════════
   📁  STEP 2 — YOUR PAST CONTENT (VOICE ANCHOR)
═══════════════════════════════════════════════

   I'm going to pull your public content so the

   agent learns your exact voice.

   Drop your handles below — paste any you have,

   skip any you don't:

      X / Twitter:        @handle  (or skip)

      YouTube:            channel URL  (or skip)

      Instagram:          @handle  (or skip)

      TikTok:             @handle  (or skip)

   Just paste them in one message — I'll figure it out.

═══════════════════════════════════════════════

```

Wait for the user's reply. Parse out whatever handles they give.

## Per-platform pull (run only for handles provided)

For each platform, pull and save independently. **If a platform fails, log it and move on — never block.**

### X / Twitter

1. Try public nitter RSS:
   ```
   curl -s -A "Mozilla/5.0" "https://nitter.net/<handle>/rss" -o /tmp/x.rss --max-time 15
   ```
2. Parse RSS items into JSON. Save to `~/himoozai/data/tweets.json`:
   ```json
   { "handle": "@handle", "fetched": "...", "source": "nitter rss", "count": N, "tweets": [{"id":"...", "date":"...", "link":"...", "text":"..."}] }
   ```
3. If nitter fails, tell user: "X didn't pull cleanly — drop your X archive zip later. Moving on."

### YouTube

1. Ensure `yt-dlp` installed (`which yt-dlp`). If not, `brew install yt-dlp`.
2. Pull recent 15 transcripts:
   ```
   cd ~/himoozai/data/youtube && yt-dlp --write-auto-sub --skip-download \
     --sub-format vtt --sub-lang en --output "%(id)s.%(ext)s" \
     --playlist-end 15 "<channel-url>"
   ```
3. Convert each `.vtt` to cleaned `.txt` (strip timestamps, dedupe lines). Delete the `.vtt` after.
4. If no auto-captions, log and move on.

### Instagram

1. Try `yt-dlp` for IG (works for some public accounts):
   ```
   yt-dlp --skip-download --write-info-json --playlist-end 15 \
     -o "~/himoozai/data/instagram/%(id)s.%(ext)s" \
     "https://instagram.com/<handle>"
   ```
2. Parse the resulting `.info.json` files for `description` (caption text).
3. Save to `~/himoozai/data/instagram/posts.json`:
   ```json
   { "handle": "@handle", "fetched": "...", "count": N, "posts": [{"id":"...", "caption":"...", "url":"..."}] }
   ```
4. If IG blocks, tell user and move on.

### TikTok

1. Try `yt-dlp` for TikTok:
   ```
   yt-dlp --skip-download --write-info-json --playlist-end 15 \
     -o "~/himoozai/data/tiktok/%(id)s.%(ext)s" \
     "https://tiktok.com/@<handle>"
   ```
2. Parse `.info.json` for description / video text.
3. Save to `~/himoozai/data/tiktok/posts.json` (same shape as IG).
4. If it fails, log and move on.

## Show progress

```
   🔄  Pulling X tweets...     ✅  20 tweets
   🔄  Pulling YouTube...      ✅  15 transcripts
   🔄  Pulling Instagram...    ✴️  blocked, skipped
   🔄  Pulling TikTok...       ✅  12 captions
```

## On completion

If at least ONE platform succeeded:

1. Update `~/.claude/himoozai-setup.json`:
   - `data_pulled: true`
   - `platforms_loaded: ["x", "youtube", ...]`

2. Show:

```

═══════════════════════════════════════════════
   ✅  STEP 2 COMPLETE — YOUR VOICE IS LOCKED
═══════════════════════════════════════════════

   ✅  X:           N tweets

   ✅  YouTube:     M transcripts

   ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░  50%

═══════════════════════════════════════════════

   Next up: load 3–5 creators you want to sound like.

   Their pieces become the structural anchors — the

   agent infuses YOUR voice into THEIR style.

   Run /himoozai to continue.

```

If ALL fail, set `data_pulled: true` anyway (don't trap the user) but warn that voice will be generic.

## Hard rules

- **NEVER analyse their content** — just count and store
- **NEVER suggest content changes** — this skill ingests, nothing else
- **NEVER block on failure** — each platform independent

## What this skill does NOT do

- Doesn't ask for app connections (public-scrape-only)
- Doesn't pull inspiration creators (that's the next skill)
- Doesn't generate content
