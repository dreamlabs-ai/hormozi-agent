---
name: hormozi-backdata
description: >
  Pulls the user's past content from public sources (X, YouTube, Instagram, TikTok)
  into ~/hormozi/data/. No MCP / app-connection required — pure public scrape.
  Each platform is independent; if one fails, others still complete. Marks
  data_pulled=true when at least one platform succeeds. Invoked only by the master
  /hormozi skill.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
---

# Hormozi Backdata — Pull Their Past Content

You pull the user's public content into `~/hormozi/data/`. The agent uses this content as voice training material on every future generation.

## The single question (no menus, no choices)

Show this:

```

═══════════════════════════════════════════════
   📁  STEP 2 — YOUR PAST CONTENT
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

Wait for the user's reply. Parse out whatever handles they give you.

## Per-platform pull (run only for handles they provided)

For each platform the user gave you, pull and save independently. **If a platform fails, log it and move on — never block.**

### X / Twitter

1. Try the public nitter RSS first:
   ```
   curl -s -A "Mozilla/5.0" "https://nitter.net/<handle>/rss" -o /tmp/x.rss --max-time 15
   ```
2. Parse the RSS items into JSON:
   - `id`, `date`, `link`, `text` per tweet
3. Save to `~/hormozi/data/tweets.json`:
   ```json
   {
     "handle": "@handle",
     "fetched": "YYYY-MM-DD",
     "source": "nitter rss",
     "count": N,
     "tweets": [...]
   }
   ```
4. If nitter returns nothing or errors out, tell the user: "X didn't pull cleanly — you can drop your X archive zip later via `/hormozi`. Moving on." Do NOT block.

### YouTube

1. Ensure `yt-dlp` is installed (`which yt-dlp`). If not, run `brew install yt-dlp` and continue.
2. Pull the most recent 15 transcripts from the channel:
   ```
   cd ~/hormozi/data/youtube && yt-dlp --write-auto-sub --skip-download \
     --sub-format vtt --sub-lang en --output "%(id)s.%(ext)s" \
     --playlist-end 15 "<channel-url>"
   ```
3. Convert each `.vtt` → cleaned `.txt` (strip timestamps, dedupe lines):
   - Use Python with regex to parse the VTT
   - Save as `~/hormozi/data/youtube/<videoId>.txt`
   - Delete the original `.vtt`
4. If the channel has no auto-captions (subtitle download warns "no subtitles"), tell the user and move on.

### Instagram

1. Try `yt-dlp` for IG too — it can pull post metadata + captions for public profiles:
   ```
   yt-dlp --skip-download --write-info-json --playlist-end 15 \
     -o "~/hormozi/data/instagram/%(id)s.%(ext)s" \
     "https://instagram.com/<handle>"
   ```
2. Parse the resulting `.info.json` files for `description` / caption text.
3. Save to `~/hormozi/data/instagram/posts.json`:
   ```json
   { "handle": "@handle", "fetched": "...", "count": N, "posts": [{"id":"...", "caption":"...", "url":"..."}] }
   ```
4. If IG blocks the request (very common), tell the user: "IG blocked the public scrape — you can paste captions later. Moving on."

### TikTok

1. Try `yt-dlp` for TikTok:
   ```
   yt-dlp --skip-download --write-info-json --playlist-end 15 \
     -o "~/hormozi/data/tiktok/%(id)s.%(ext)s" \
     "https://tiktok.com/@<handle>"
   ```
2. Parse `.info.json` for `description` / video text.
3. Save to `~/hormozi/data/tiktok/posts.json` in the same shape as IG.
4. If it fails, log and move on.

## Show progress as you go

For each platform, show ONE line of status:

```
   🔄  Pulling X tweets...     ✅  20 tweets
   🔄  Pulling YouTube...      ✅  15 transcripts
   🔄  Pulling Instagram...    ✴️  blocked, skipped
   🔄  Pulling TikTok...       ✅  12 captions
```

## On completion

If at least ONE platform succeeded:

1. Update `~/.claude/hormozi-setup.json`:
   - `data_pulled: true`
   - `platforms_loaded: ["x", "youtube", ...]` (only the ones that worked)

2. Show:

```

═══════════════════════════════════════════════
   🎉  STEP 2 COMPLETE — YOU'RE READY
═══════════════════════════════════════════════

   ✅  X:           N tweets

   ✅  YouTube:     M transcripts

   ✅  Instagram:   K captions

   ✅  TikTok:      P captions

   ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░  67%

═══════════════════════════════════════════════

   You're ready to make content.

   Run /hormozi any time — every run is a fresh

   content session. Type → models → idea → variants.

   Let's make something. Run /hormozi now.

```

## If ALL platforms fail

Set `data_pulled: true` anyway (don't trap the user) but show:

> ✴️  No platforms pulled cleanly. The agent will work without back data — content will be generic. Re-run /hormozi backdata later if you want to retry, or paste your X archive zip.

## Hard rules

- **NEVER analyse their content.** Don't comment on engagement, hooks, performance — just count and store.
- **NEVER suggest content changes.** This skill ingests, nothing else.
- **NEVER block on failure.** Each platform is independent. Move on.

## What this skill does NOT do

- Doesn't ask for app connections / MCPs (this agent is public-scrape-only)
- Doesn't analyse the content
- Doesn't generate content
- Doesn't loop creators of inspiration — that's not part of the v0.2 flow
