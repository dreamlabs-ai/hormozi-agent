# Push to GitHub — One-Time Setup

The local repo is ready at `~/hormozi-agent/`. Here's how to get it onto GitHub as `dreamlabs-ai/hormozi-agent` so the install commands in the README actually work.

## 1. Create the GitHub org (if it doesn't exist)

Go to **github.com/organizations/new** and create a free org named `dreamlabs-ai`.

## 2. Create the empty repo

Go to **github.com/organizations/dreamlabs-ai/repositories/new** and create a public repo named `hormozi-agent`. **Do not** initialize with README/license — the local repo already has one.

## 3. Wire up and push

From this terminal:

```bash
cd ~/hormozi-agent
git commit -m "Initial commit — Hormozi Agent v0.1.0"
git remote add origin https://github.com/dreamlabs-ai/hormozi-agent.git
git push -u origin main
```

If you'd rather push from your personal account first to test, swap the URL for `https://github.com/benjaminjkw/hormozi-agent.git` and update the README's install line accordingly. You can transfer the repo to `dreamlabs-ai` later from GitHub's repo settings.

## 4. Smoke test the install

In a fresh Claude Code session:

```
/plugin marketplace add dreamlabs-ai/hormozi-agent
/plugin install hormozi@hormozi-agent
/hormozi
```

You should see the progress board with all 6 steps as ⬜ and 0%. Walk through `/hormozi onboard` end-to-end and verify:

- `~/hormozi/business.md` gets created with your 12 answers
- `~/.claude/hormozi-setup.json` flips `step1_onboard: true`

If it works, you're ready to film.

## What ships in v0.1.0

| File | Purpose |
|---|---|
| `.claude-plugin/marketplace.json` | Marketplace manifest |
| `hormozi/.claude-plugin/plugin.json` | Plugin metadata |
| `hormozi/skills/hormozi/SKILL.md` | Master entry — `/hormozi` |
| `hormozi/skills/hormozi-onboard/SKILL.md` | 12 questions → business.md |
| `hormozi/skills/hormozi-connect/SKILL.md` | App connection wizard |
| `hormozi/skills/hormozi-backdata/SKILL.md` | Tweets + YouTube transcripts |
| `hormozi/skills/hormozi-inspiration/SKILL.md` | Creator voice patterns |
| `hormozi/skills/hormozi-idea/SKILL.md` | Idea variant generator |
| `hormozi/skills/hormozi-generate/SKILL.md` | HTML script generator |
| `hormozi/agents/hormozi-writer.md` | Sonnet subagent — actual writer |
| `hormozi/templates/business-md.template` | 12-question scaffold |
| `hormozi/templates/script.html.template` | Dark-aesthetic HTML output |

## Things to verify before filming

- [ ] Repo is public on GitHub
- [ ] `/plugin marketplace add dreamlabs-ai/hormozi-agent` succeeds
- [ ] `/plugin install hormozi@hormozi-agent` succeeds
- [ ] `/hormozi` runs and shows the progress board
- [ ] `/hormozi onboard` walks through all 12 questions and writes `business.md`
- [ ] `/hormozi generate "test idea"` produces an HTML file at `~/hormozi/output/`
- [ ] The HTML opens cleanly in your browser

## Things to add post-video (v0.2.0)

- Instagram caption ingestion in `hormozi-backdata`
- Blog/newsletter URL ingestion in `hormozi-backdata`
- A `hormozi-analyse` skill that runs the full Hormozi business analysis on `business.md`
- Auto-detect when `yt-dlp` isn't installed and prompt the install
- Cron-style auto-refresh of back data
