# Himoozai Agent

A drop-in Claude Code plugin that turns your terminal into a Hormozi-method content factory. Authentic, on-brand tweets, YouTube scripts, Instagram Reels, TikTok scripts, and email newsletters — at the click of a button.

## Install

Open Claude Code and paste these three lines:

```
/plugin marketplace add dreamlabs-ai/himoozai-agent
/plugin install himoozai@himoozai-agent
/himoozai
```

That's it. The wizard takes over from there.

## How it works

Locked linear setup, then unlimited content sessions:

1. **Onboard** — 12 questions → `~/himoozai/business.md` (your voice, avatar, offer)
2. **Back Data** — pulls your past content from X / YouTube / Instagram / TikTok (public scrape, no app connections). This becomes your **voice anchor** on every generation.
3. **Inspiration** — load 3–5 creators you want to make content like. Their content becomes the **structural anchor** at create-time.

After setup, pick a platform command:

```
/himoozai-x             Tweet / X thread        → 10 variants
/himoozai-yt            YouTube script          →  3 variants
/himoozai-ig            Instagram Reel script   →  5 variants
/himoozai-tt            TikTok script           →  5 variants
/himoozai-newsletter    Email newsletter        →  3 variants
```

Each one runs the same locked sub-flow:

> pick 1–5 inspiration pieces → submit your idea → variants returned → save the chosen one

The agent infuses **YOUR voice** into the **inspiration's structure**, governed by Hormozi's actual published content prompts (baked in per platform).

## What's pre-installed

The plugin ships with the real Hormozi-method prompts for each platform:

- **Tweet Style Analyzer** (calibrated against 5,080 @AlexHormozi tweets)
- **YouTube Script Style Extractor**
- **Short-Form Content Extractor** (TikTok / IG Reels)
- **Newsletter Style Analyzer**

You don't paste prompts. You don't write prompts. You give an idea, you get content.

## What this agent does NOT do

- ❌  Doesn't give business advice
- ❌  Doesn't analyse your offer, pricing, or strategy
- ❌  Doesn't suggest improvements to your business
- ✅  Only makes content. Forever.

## Files it creates on your machine

```
~/.claude/himoozai-setup.json     # Setup progress
~/himoozai/business.md            # The 12 answers (your voice + offer)
~/himoozai/data/                  # Your past content (voice anchor)
~/himoozai/inspiration/           # Creators you model (style anchors)
~/himoozai/output/                # Saved generations (one .md per piece)
```

Everything is local. Edit `business.md` by hand any time.

## Built by

[Dream Labs AI](https://github.com/dreamlabs-ai). The method is Alex Hormozi's. Credit to him.
