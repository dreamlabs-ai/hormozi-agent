---
name: hormozi-backdata
description: >
  Pulls the user's past content into ~/hormozi/data/ so the agent can mimic their voice.
  Two independent sub-flows: tweets (X handle scrape OR archive zip) and YouTube
  (channel URL or list of video URLs → transcripts). User can do one, both, or skip.
  Marks step3_backdata true once at least one source has data.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
---

# Hormozi Backdata — Tweets + YouTube

You pull the user's past content into local files. Follow the dreamlabs-style rules.

## Open

```

═══════════════════════════════════════════════
   📁  BACK DATA — YOUR VOICE
═══════════════════════════════════════════════

   Your agent gets sharper when it can see how you actually talk.

   Pick what to feed in:

   1.  Tweets / X archive

   2.  YouTube transcripts

   3.  Both

   4.  Skip — I'll do this later

═══════════════════════════════════════════════

```

## Sub-flow A — Tweets

Two paths, ask which:

### A1. They paste an X handle

1. Run a fetcher to get their public tweets. Try in this order:
   - `npx -y @the-convocation/twitter-scraper@latest <handle>` if available
   - Otherwise: `curl -s "https://nitter.net/<handle>/rss"` and parse the RSS
   - If neither works, fall back to A2 (archive upload)

2. Save normalized JSON to `~/hormozi/data/tweets.json`:

```json
{
  "handle": "@username",
  "fetched": "2026-05-02",
  "count": 287,
  "tweets": [
    {"id": "...", "date": "2026-04-15", "text": "..."}
  ]
}
```

### A2. They have an X archive zip

1. Ask them to download their X archive (Settings → Your account → Download an archive of your data).
2. Once they have it, ask for the absolute path to the `.zip` or unzipped folder.
3. Find `data/tweets.js` or `data/tweet.js` in the archive. Parse it (it's JSON wrapped in a JS variable assignment — strip the leading `window.YTD.tweets.part0 = ` or similar).
4. Save to `~/hormozi/data/tweets.json` in the same shape as A1.

After either path: `🎉 Pulled N tweets into ~/hormozi/data/tweets.json`

## Sub-flow B — YouTube

1. Ask: "Paste a channel URL OR a list of video URLs (one per line)."

2. Get the video list:
   - For a channel URL: use `yt-dlp --flat-playlist -J <url>` to get every video ID. Take the most recent 50 by default — confirm with user if they want more or fewer.
   - For a video list: parse out the video IDs.

3. For each video ID, pull the transcript:
   - `yt-dlp --write-auto-sub --skip-download --sub-format vtt --output "%(id)s" <url>` then read the `.vtt` and strip timestamps
   - Save the cleaned plaintext to `~/hormozi/data/youtube/<videoId>.txt`

4. If `yt-dlp` isn't installed, tell the user to run `brew install yt-dlp` (macOS) or `pip install yt-dlp`. Offer to wait.

5. Show progress as you go: `▓▓▓░░░░░░ 12 / 50 transcripts pulled`

After: `🎉 Pulled N transcripts into ~/hormozi/data/youtube/`

## On completion

If at least ONE source has data, update `~/.claude/hormozi-setup.json` — set `step3_backdata: true`.

```

═══════════════════════════════════════════════
   ✅  BACK DATA INGESTED
═══════════════════════════════════════════════

   📁  ~/hormozi/data/tweets.json     (N tweets)

   📁  ~/hormozi/data/youtube/         (M transcripts)

   Ready for **Step 4: Inspiration**?

   That's where we feed in creators you want to make content like.

   Type /hormozi inspiration to start.

═══════════════════════════════════════════════

```

## Tone

Practical. If a tool is missing, give the exact install command and move on. Don't apologise — direct and helpful.

## What this skill does NOT do

- Doesn't post anywhere
- Doesn't analyse the content (that happens in idea/generate)
- Doesn't pull non-public content (no X DMs, no private YT)
