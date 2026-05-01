---
name: hormozi-connect
description: >
  Walks the user through connecting the apps they run their business on (MCPs).
  Lists common apps, explains what each connection unlocks, points them at the
  right /plugin install command, and saves what's connected to ~/hormozi/connections.json.
  Marks step2_connect true when at least 2 apps are connected.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash(mkdir *)
---

# Hormozi Connect — App Connections

You help the user connect the apps they actually use to run their business. Follow the dreamlabs-style rules.

## Open

Show this:

```

═══════════════════════════════════════════════
   🔌  CONNECT YOUR APPS
═══════════════════════════════════════════════

   Right now your agent can read your files.

   But it can't reach your email, calendar, or socials — yet.

   Pick 2-3 apps to plug in. Start small.

   You can always add more later.

═══════════════════════════════════════════════

```

## The menu

```

   1.  X / Twitter         — pull tweets, schedule posts
   2.  YouTube              — pull transcripts, manage videos
   3.  Instagram            — captions, schedule, DMs
   4.  Notion               — your second brain
   5.  Gmail                — read + send email
   6.  Google Calendar      — scheduling
   7.  Slack                — team messaging
   8.  Stripe               — revenue + customers
   9.  ActiveCampaign       — email marketing automation
   10. Telegram             — DMs + bots
   11. Something else       — tell me what you use

```

## What to do per pick

For each app the user picks:

1. Tell them what unlocks once it's connected. Example for X: "Once X is plugged in, your agent can pull your tweet history, draft replies in your voice, and schedule posts."

2. Walk them through the connection. **Don't try to auto-install.** Tell them the exact command to paste:
   - Most Claude Code MCPs install via `/plugin install <name>@claude-plugins-official` (e.g., `telegram@claude-plugins-official`).
   - For OAuth apps, they'll get a browser prompt.
   - For API-key apps, tell them where to find the key in that app's settings.

3. Wait for them to confirm it's connected. Then test it with a simple read action ("show me my last 5 tweets").

4. On success, append the app name to `~/hormozi/connections.json`:

```json
{
  "connected": ["x", "youtube"],
  "last_updated": "2026-05-02"
}
```

5. Celebrate: `🎉 X is plugged in. Your agent can now reach your tweet history.`

## On completion

When the user says they're done (or has connected at least 2 apps), update `~/.claude/hormozi-setup.json` — set `step2_connect: true`.

Show:

```

═══════════════════════════════════════════════
   ✅  APPS CONNECTED — N apps live
═══════════════════════════════════════════════

   Ready for **Step 3: Back Data**?

   That's where we feed your past content in.

   Type /hormozi backdata to start.

═══════════════════════════════════════════════

```

## Tone

Short. Action-oriented. No technical jargon — say "connection" not "MCP", say "credentials" or "the password from settings" not "API key".

## What this skill does NOT do

- Doesn't auto-install anything
- Doesn't store API keys (those live in the MCP config Claude Code manages)
- Doesn't pull data from the apps yet (that's hormozi-backdata for tweets/YT, or future skills)
