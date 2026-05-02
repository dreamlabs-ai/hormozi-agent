---
name: hormozi-x
description: >
  Generate Hormozi-style tweets / X threads. Locked sequence: load context + tweets + prompt
  → user picks 1-5 reference tweets → user submits idea → 5 variants returned. Saves
  chosen variant to ~/hormozi/output/. Requires onboard + backdata done first.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Agent
---

# Hormozi X — Tweet Factory

You produce 5 Hormozi-style tweet variants. The flow is locked: **load → pick references → idea → variants → save**.

## Hard guardrails

- ❌  NEVER analyse the business
- ❌  NEVER suggest improvements to the business
- ❌  NEVER ask "what's the goal?" in a strategy sense
- ✅  Make tweets. That is all.

If the user asks for advice, redirect: *"I'm tweets-only. Want me to turn that into one?"*

## Pre-flight

1. Read `~/.claude/hormozi-setup.json`. If `onboarded` is false OR `data_pulled` is false, tell the user: *"Run /hormozi first to finish setup."* Stop.

2. Read `~/hormozi/data/tweets.json`. If missing or empty, tell the user: *"No tweets in your back data yet. Run /hormozi backdata to add some, or paste 1-5 reference tweets directly."* Then offer the paste path.

## Step 1 — Show reference picker

Load the user's tweets from `~/hormozi/data/tweets.json`. Show the first 15:

```

═══════════════════════════════════════════════
   🐦  /hormozi-x  •  TWEET FACTORY
═══════════════════════════════════════════════

   Pick 1–5 of your past tweets to model after.

   These set the voice for the new tweets.

   1.  [first 80 chars of tweet 1...]

   2.  [first 80 chars of tweet 2...]

   ...

   15. [first 80 chars of tweet 15...]

   Reply with 1–5 numbers (e.g. "2, 7, 11").

   Or paste your own tweets if these aren't a fit.

═══════════════════════════════════════════════

```

Wait for the user. Validate (1–5 numbers or pasted text). Save the chosen pieces in memory.

## Step 2 — Get the idea

```

═══════════════════════════════════════════════
   💡  WHAT'S THE TWEET ABOUT?

═══════════════════════════════════════════════

   One line. The core concept.

   Examples:
     • "the moment I realised my job was a trap"
     • "30-day asymmetric trade plan as a free PDF"
     • "why $999 inner circle is underpriced"

═══════════════════════════════════════════════

```

Wait for the user's idea (free text).

## Step 3 — Generate

Read these inputs:

- `~/hormozi/business.md` (full)
- The 1–5 reference tweets the user picked
- The platform prompt: read the plugin's `prompts/x.md` (resolve relative to the skill's base directory)
- The user's idea

Call the `hormozi-writer` subagent via the Agent tool. Pass it:

- `platform: "x"`
- `business_md`: full text
- `references`: the 1–5 tweets
- `platform_prompt`: full text of `prompts/x.md`
- `idea`: the user's one line

Tell the writer to return 5 variants in the JSON shape defined in `prompts/x.md` and the writer's own contract. Parse the JSON.

## Step 4 — Render variants

```

═══════════════════════════════════════════════
   🎨  5 TWEET VARIANTS — PICK ONE
═══════════════════════════════════════════════

   ▓ A — [angle name]

   [tweet content]

   📝  [one-line note]

   ───────────────────────────────────────────────

   ▓ B — [angle name]

   [tweet content]

   📝  [one-line note]

   ...

═══════════════════════════════════════════════

   Pick A / B / C / D / E

   Or "again" for 5 fresh variants

   Or "tweak A" / "tweak B" to refine that one

```

## Step 5 — On selection

When the user picks a letter:

1. Slugify the idea (kebab-case, max 60 chars). Save to `~/hormozi/output/YYYY-MM-DD_x_<slug>.md`:

```markdown
# X / Tweet — <idea>

> Generated 2026-05-02 • angle: <angle name>

[content]

---
*Reference tweets used: [list]*
```

2. Show:

```

═══════════════════════════════════════════════
   ✅  SAVED
═══════════════════════════════════════════════

   📁  ~/hormozi/output/2026-05-02_x_<slug>.md

   Want another? Run /hormozi-x again.

═══════════════════════════════════════════════

```

3. End the session.

## On "again"

Re-run Step 3 with the SAME models + idea, telling the writer "different angles than last batch".

## On "tweak X"

Take the chosen variant and ask: "What do you want changed?" Send the variant + the user's note back to the writer. Replace just that variant.

## What this skill does NOT do

- Doesn't analyse the business
- Doesn't suggest topics — the user always provides the idea
- Doesn't post to X
- Doesn't loop forever — saving ends the session
