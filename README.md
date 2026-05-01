# Hormozi Agent

A drop-in Claude Code plugin that turns your terminal into a Hormozi-style business + content agent.

## Install

Open Claude Code and paste these three lines:

```
/plugin marketplace add dreamlabs-ai/hormozi-agent
/plugin install hormozi@hormozi-agent
/hormozi
```

That's it. The wizard takes over from there.

## What it does

1. **Onboards your business** — walks you through Hormozi's 12 questions and saves the answers to `~/hormozi/business.md`
2. **Connects your apps** — wires up the MCPs you run your business on (X, YouTube, Notion, Gmail, Slack, etc.)
3. **Pulls your back data** — your tweets and YouTube transcripts so the agent learns *your voice*
4. **Pulls your inspiration** — creators you want to make content like
5. **Generates ideas** — `/hormozi idea "topic"` gives you 5–10 idea variants
6. **Generates scripts** — `/hormozi generate "idea"` outputs a screen-record-friendly HTML script

## Files it creates on your machine

```
~/.claude/hormozi-setup.json     # Wizard progress
~/hormozi/business.md            # The 12 answers
~/hormozi/connections.json       # Which MCPs are wired up
~/hormozi/data/                  # Your tweets + YT transcripts
~/hormozi/inspiration/           # Creators you admire
~/hormozi/output/                # Generated HTML scripts
```

Everything is local. Edit `business.md` by hand any time.

## Built by

[Dream Labs AI](https://github.com/dreamlabs-ai) — running on the back of Alex Hormozi's frameworks. All credit to Alex.
