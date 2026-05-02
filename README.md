# Hormozi Agent

A drop-in Claude Code plugin that turns your terminal into a Hormozi-grade content factory. Authentic, on-brand tweets, IG posts, YouTube scripts, TikTok scripts, and emails — at the click of a button.

## Install

Open Claude Code and paste these three lines:

```
/plugin marketplace add dreamlabs-ai/hormozi-agent
/plugin install hormozi@hormozi-agent
/hormozi
```

That's it. The wizard takes over from there.

## How it works

The agent runs in a **locked linear sequence** — every step always follows the last:

1. **Onboard** — 12 questions to lock in your voice, avatar, and offer (saved to `~/hormozi/business.md`)
2. **Back Data** — pulls your past content from X / YouTube / Instagram / TikTok via public scrape (no app connections needed)
3. **Create** — pick a platform command and run a content session

The 5 platform commands:

```
/hormozi-x             Tweet / X thread        →  5 variants
/hormozi-yt            YouTube script          →  3 variants
/hormozi-ig            Instagram caption       →  5 variants
/hormozi-tt            TikTok script           →  5 variants
/hormozi-newsletter    Email newsletter        →  3 variants
```

Each one runs the same locked sub-flow: **load context + your data + the platform's pre-installed Hormozi prompt → pick 1-5 reference pieces from your past content → submit your idea → variants returned → save the chosen one.**

It does **content only**. It will never give business advice, analyse your offer, or comment on your strategy.

## Files it creates on your machine

```
~/.claude/hormozi-setup.json     # Wizard progress (2 flags: onboarded, data_pulled)
~/hormozi/business.md            # The 12 answers
~/hormozi/data/                  # Your tweets, YT transcripts, IG, TikTok captions
~/hormozi/output/                # Saved generations, one .md per piece
```

Everything is local. Edit `business.md` by hand any time.

## Built by

[Dream Labs AI](https://github.com/dreamlabs-ai) — running on the back of Alex Hormozi's frameworks. All credit to Alex.
