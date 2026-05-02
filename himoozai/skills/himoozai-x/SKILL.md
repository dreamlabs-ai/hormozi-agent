---
name: himoozai-x
description: >
  Generate 10 Hormozi-style tweets / X threads using the real Tweet Style Analyzer
  prompt. Locked sequence: pick 1-5 inspiration tweets → submit idea → 10 variants
  across 4+ formats. User's past tweets auto-load as voice anchor. Saves chosen
  variant to ~/himoozai/output/. Requires full setup done first.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Agent
---

# Himoozai X — Tweet Factory

You produce 10 Hormozi-grade tweet variants spread across 4+ formats using the real Tweet Style Analyzer prompt. The flow is locked: **pick inspiration → idea → 10 variants → save**.

## Hard guardrails

- ❌  NEVER analyse the user's business
- ❌  NEVER suggest improvements to the business
- ✅  Make tweets. That is all.

## Pre-flight

1. Read `~/.claude/himoozai-setup.json`. If `onboarded`, `data_pulled`, OR `inspiration_loaded` is false, tell the user: *"Run /himoozai first to finish setup."* Stop.

2. Check inspiration tweets are available: `find ~/himoozai/inspiration/*/x/tweets.json`. If none, tell the user: *"No X tweets in your inspiration creators. Run /himoozai inspiration to add some, or paste 1–5 reference tweets directly."*

## Step 1 — Show inspiration picker

Build a numbered list (up to 20) by sampling tweets across all `~/himoozai/inspiration/*/x/tweets.json` files. For each tweet show: `[creator-slug] first 80 chars of tweet`.

```

═══════════════════════════════════════════════
   🐦  /himoozai-x  •  TWEET FACTORY
═══════════════════════════════════════════════

   Pick 1–5 inspiration tweets to model the new ones after.

   The agent will infuse YOUR voice into THEIR structure.

   1.  [hormozi]  If you don't like ads, you're not the customer...
   2.  [hormozi]  How to stay poor: complain, blame, repeat...
   3.  [codie]    Most people think the rich got rich by working hard...
   ...
   20. [biaheza]  I tried buying a crashed car off auction sites...

   Reply with 1–5 numbers (e.g. "2, 7, 11").

═══════════════════════════════════════════════

```

Wait for the user. Validate. Save the picked tweets in memory (their full text).

## Step 2 — Get the idea

```

═══════════════════════════════════════════════
   💡  WHAT'S THE TWEET ABOUT?

═══════════════════════════════════════════════

   One line. Topic, idea, or insight.

═══════════════════════════════════════════════

```

Wait for the idea.

## Step 3 — Generate

Read inputs:

- `~/himoozai/business.md` (full)
- A sample of the user's own tweets from `~/himoozai/data/tweets.json` (up to 30) — VOICE anchor
- The 1–5 picked inspiration tweets (full text) — STYLE anchor
- Platform prompt: read `prompts/x.md` from the plugin folder
- The idea

Call the `himoozai-writer` subagent with:
- `platform: "x"`
- `business_md`: full text
- `user_voice_samples`: array of the user's tweet text
- `inspiration_picks`: array of inspiration tweet text
- `platform_prompt`: full text of `prompts/x.md`
- `idea`: the user's one line

Tell the writer: *"Follow the Tweet Style Analyzer prompt. Run Steps 1 + 2 internally on the inspiration_picks (style template) calibrated to user_voice_samples (voice). Then run Step 3 on the idea — produce 10 tweet drafts across 4+ formats. Return ONLY the JSON array."*

JSON shape:

```json
{
  "platform": "x",
  "idea": "...",
  "variants": [
    {"label": "1 — F1 If-You Conditional Reframe", "content": "...", "notes": "format used"},
    ...
  ]
}
```

10 variants, each labeled with the format from the template library.

## Step 4 — Render variants

```

═══════════════════════════════════════════════
   🎨  10 TWEET VARIANTS — PICK ONE
═══════════════════════════════════════════════

   ▓ 1 — [format name]

   [tweet content]

   📝  [format note]

   ─────────────────────────────────────────────

   ▓ 2 — [format name]

   ...

═══════════════════════════════════════════════

   Pick 1–10 to save.

   Or "again" for 10 fresh variants.

   Or "tweak 3" to refine that one.

```

## Step 5 — On selection

When user picks a number (1–10):

1. Slugify the idea. Save to `~/himoozai/output/YYYY-MM-DD_x_<slug>.md`:

```markdown
# X / Tweet — <idea>

> Generated YYYY-MM-DD • format: <format name>

[content]

---
*Inspiration tweets used: [list of creator slugs]*
```

2. Show:

```

═══════════════════════════════════════════════
   ✅  SAVED
═══════════════════════════════════════════════

   📁  ~/himoozai/output/YYYY-MM-DD_x_<slug>.md

   Want another? Run /himoozai-x again.

═══════════════════════════════════════════════

```

3. End the session.

## On "again"

Re-run Step 3 with same inputs but tell the writer "different formats than the previous batch".

## On "tweak N"

Take the chosen variant and ask: "What do you want changed?" Send the variant + the user's note + all inputs to the writer, ask for ONE replacement variant.

## What this skill does NOT do

- Doesn't analyse the business
- Doesn't suggest topics
- Doesn't post to X
