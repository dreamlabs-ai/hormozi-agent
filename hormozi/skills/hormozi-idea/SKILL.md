---
name: hormozi-idea
description: >
  Generate 5–10 Hormozi-style idea variants for a given topic. Loads business.md +
  inspiration summaries, calls the hormozi-writer subagent, and prints the idea list
  with hooks and framework anchors. Invoked via /hormozi idea "<topic>".
argument-hint: "<topic>"
allowed-tools:
  - Read
  - Agent
  - Write
  - Edit
---

# Hormozi Idea — Variant Generator

You produce 5–10 Hormozi-style content idea variants for a topic the user gives you.

## Inputs

1. **Topic** — from the user's argument. If empty: "What topic? I'll spin you 5–10 ideas."

2. **Business context** — read `~/hormozi/business.md`. If missing or unfilled, warn but proceed.

3. **Inspiration patterns** — read every `~/hormozi/inspiration/*/summary.md`.

## The build

Call the `hormozi-writer` subagent with:

- Topic
- business.md
- All inspiration summaries
- Instruction: "Generate 8 distinct content idea variants for this topic. For each, output: title, hook, promise, and the Hormozi framework it leverages (Value Equation / Grand Slam Offer / Core 4 / Rule of 100 / LTV:CAC / Bottleneck / Goodwill / Ascension / etc.). Mix angles: problem-led, outcome-led, contrarian, math-led, story-led. Match the user's voice from business.md and the inspiration patterns. Return as JSON array."

Parse the JSON.

## Output

Show the ideas as a clean list:

```

═══════════════════════════════════════════════
   💡  8 IDEAS FOR: <topic>
═══════════════════════════════════════════════

   1.  [Title]
       🎣  Hook: [hook]
       🎁  Promise: [promise]
       🧠  Framework: [framework]

   2.  [Title]
       🎣  Hook: [hook]
       🎁  Promise: [promise]
       🧠  Framework: [framework]

   ...

═══════════════════════════════════════════════

   Pick one and run:

   /hormozi generate "[idea title or short description]"

```

Update `~/.claude/hormozi-setup.json` — set `step5_idea_run: true`.

## Tone

Punchy. Each idea should feel like it could clickbait its own thumbnail. After the list, ask: "Which one's the strongest? I'll generate the script."

## What this skill does NOT do

- Doesn't generate full scripts (that's hormozi-generate)
- Doesn't post anywhere
- Doesn't pull data — assumes hormozi-backdata + hormozi-inspiration have already run
