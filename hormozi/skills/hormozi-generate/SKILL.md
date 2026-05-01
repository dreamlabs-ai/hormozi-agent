---
name: hormozi-generate
description: >
  Generate a full Hormozi-style content script as a single-file HTML doc.
  Loads ~/hormozi/business.md + back data + inspiration summaries, calls the
  hormozi-writer subagent for the actual writing, fills the script.html.template,
  and writes the output to ~/hormozi/output/YYYY-MM-DD_<slug>.html. Prints a
  file:// URL when done. Invoked via /hormozi generate "<idea>".
argument-hint: "<idea or topic>"
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash(mkdir *)
  - Bash(date *)
  - Bash(open *)
  - Agent
---

# Hormozi Generate — HTML Script Output

You produce a screen-record-friendly HTML script using the Hormozi frameworks + the user's voice.

## Inputs

1. **The idea** — from the user's argument. If empty, prompt: "What's the idea or topic? I'll turn it into a script."

2. **Business context** — read `~/hormozi/business.md`. If missing or template placeholders are still in it, warn the user: "⚠️ Your business context isn't filled in yet. Run `/hormozi onboard` first or the script will be generic." Offer to continue anyway.

3. **Voice samples** — read up to ~30 entries from `~/hormozi/data/tweets.json` and up to 3 transcript files from `~/hormozi/data/youtube/`. Pick a representative sample, not the whole archive. If neither exists, note: "No back data yet — script will use generic voice."

4. **Inspiration patterns** — read every `summary.md` under `~/hormozi/inspiration/*/`. These contain hook/voice patterns from creators the user wants to learn from. If none, skip.

## The build

1. Slugify the idea (lowercase, kebab-case, max 60 chars). Get today's date as YYYY-MM-DD. Build the output filename: `YYYY-MM-DD_<slug>.html`.

2. Ensure `~/hormozi/output/` exists.

3. Call the `hormozi-writer` subagent via the Agent tool. Pass it:
   - The idea
   - The full `business.md` content
   - The voice samples (tweets + transcript snippets)
   - The inspiration summaries
   - Instruction: "Produce a Hormozi-style script for the idea. Output JSON with these exact keys: title, primary_framework, hook_a (problem-led), hook_b (outcome-led), hook_c (curiosity-led), promise, outline (array of {beat, body, broll}), cta, frameworks (array of strings). Match the user's voice from their samples. Use Hormozi's frameworks for structure (Value Equation, Grand Slam Offer, Core 4, Rule of 100, LTV:CAC, Bottleneck Theory, Goodwill Strategy)."

4. Parse the JSON. If parsing fails, ask the agent to retry with stricter JSON formatting.

5. Read `templates/script.html.template` (relative to the plugin root — use the plugin's path).

6. Substitute every `{{TOKEN}}`:
   - `{{TITLE}}` → title
   - `{{DATE}}` → today
   - `{{PRIMARY_FRAMEWORK}}` → primary_framework
   - `{{HOOK_A}}`, `{{HOOK_B}}`, `{{HOOK_C}}` → hooks
   - `{{PROMISE}}` → promise
   - `{{OUTLINE_ITEMS}}` → render each outline item as `<li><strong>{beat}</strong><div class="body">{body}</div><div class="broll">B-ROLL: {broll}</div></li>`
   - `{{CTA}}` → cta
   - `{{FRAMEWORK_TAGS}}` → render each framework as `<span class="framework-tag">{name}</span>`

   HTML-escape user-derived strings before substituting.

7. Write the result to `~/hormozi/output/<filename>.html`.

8. Update `~/.claude/hormozi-setup.json` — set `step6_generate_run: true`.

## Output

Show this:

```

═══════════════════════════════════════════════
   🚀  SCRIPT GENERATED
═══════════════════════════════════════════════

   📁  ~/hormozi/output/YYYY-MM-DD_<slug>.html

   🎬  file:///Users/.../hormozi/output/YYYY-MM-DD_<slug>.html

   💡  Open it in your browser to record from.

═══════════════════════════════════════════════

```

Print the actual `file:///` URL using the absolute path. The user clicks it and the script opens in their browser, screen-record ready.

Optionally offer: "Want me to open it for you?" → if yes, run `open <path>`.

## Tone

Confident. Don't apologise for using AI — Hormozi-style content is direct. After delivery, ask: "Want another angle on the same idea? Just say the word."

## Edge cases

- **No business.md:** still generate, but with generic voice. Warn the user.
- **No back data:** voice will be Hormozi-generic; warn the user.
- **No idea given:** prompt for one before doing anything.
- **Subagent JSON parse failure:** retry once with explicit "respond with valid JSON only, no prose."

## What this skill does NOT do

- Doesn't post the script anywhere
- Doesn't generate B-roll itself (just notes for cues)
- Doesn't run if the user just wants idea variants — that's `hormozi-idea`
