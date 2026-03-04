# cowork-executive-assistant

Claude Cowork skills that replace a human executive assistant.

Inspired by [mgonto/executive-assistant-skills](https://github.com/mgonto/executive-assistant-skills) (built for OpenClaw), this project reimplements the same concept natively for **Claude Cowork** — using Gmail, Google Calendar, Slack, and Google Drive as the backbone.

## What's included

| Skill | What it replaces |
|---|---|
| `meeting-prep` | EA researching attendees, pulling email history, and briefing you before each call |
| `action-items` | EA reviewing meeting notes, creating follow-up tasks, and drafting emails you promised to send |
| `email-drafting` | EA drafting replies, intro emails, scheduling responses, and thank-you notes in your voice |
| `executive-digest` | EA giving you a daily/weekly status update: stalled threads, pending intros, overdue tasks, calendar conflicts, team radar |
| `humanizer` | Making sure nothing your AI writes sounds like AI wrote it |

## How it works

Each skill is a markdown file (`SKILL.md`) that tells Claude Cowork exactly how to do the job. Claude reads the skill, follows the instructions, and delivers results via Slack DMs, Gmail drafts, or directly in the Cowork conversation.

Skills can run on cron schedules — the digest fires every morning, meeting prep runs before your first meeting, and action items process after your last meeting. You can also trigger any skill manually by asking Claude.

### Key differences from the OpenClaw version

| Feature | OpenClaw (original) | Cowork (this repo) |
|---|---|---|
| Email | gog CLI (2 Gmail accounts) | Native Gmail MCP connector |
| Calendar | — | Native Google Calendar connector |
| Chat | WhatsApp / Slack / Telegram | Native Slack connector |
| Tasks | Todoist CLI | Slack DM + Calendar events |
| Transcripts | Granola / Grain via mcporter | Gemini / Granola / Grain via Gmail/Drive |
| Delivery | WhatsApp | Slack DM, Gmail draft, or inline |
| Language | English | Any (template is PT-BR, easy to adapt) |
| Digest | Daily only | Daily (mon-thu) + Weekly (friday) |

## Prerequisites

- **Claude Desktop** with Cowork mode enabled
- The following connectors enabled in Cowork:
  - Gmail
  - Google Calendar
  - Slack
  - Google Drive (optional, for transcript search)
- Meeting transcription tool (optional) — Gemini, Granola, Grain, or Otter.ai

## Quick setup

1. Clone this repo

```bash
git clone https://github.com/YOUR_USERNAME/cowork-executive-assistant.git
```

2. Copy skills to your Cowork skills directory

```bash
cp -r cowork-executive-assistant/skills/* ~/Library/Application\ Support/Claude/skills/
```

Or ask Claude in Cowork: *"Install the skills from [path to cloned repo]"*

3. Personalize the style guide

Edit `skills/_shared/style-guide.md` with your writing style. Or take the easy route — ask Claude:

> *"Analyze my last 20 sent emails and 50 Slack messages, then fill in the style guide for me."*

4. Personalize the executive-digest

Edit `skills/executive-digest/SKILL.md` and fill in:
- Your Slack user ID and DM channel ID
- The Slack channels you want monitored
- Your peers and direct reports (with Slack user IDs)
- Your company's organizational context
- Your current pending items

5. Set up scheduled tasks

See `config/scheduled-tasks.md` for ready-to-use cron configs. Example:

```
Create a scheduled task called "executive-digest" that runs every weekday
at 8:00 AM with this prompt: [prompt from config file]
```

## What you need to personalize

The skills are designed as templates. Here's what to customize:

| File | What to change |
|---|---|
| `_shared/style-guide.md` | Your writing style, greetings, closings, anti-patterns |
| `executive-digest/SKILL.md` | Slack IDs, channels, peers, directs, org context, pending items |
| `action-items/SKILL.md` | Your Slack DM channel ID |
| `config/scheduled-tasks.md` | Cron times, channel IDs |

The other skills (`humanizer`, `email-drafting`, `meeting-prep`) work out of the box — they reference the style-guide and adapt to any user.

Look for `<!-- PERSONALIZAR -->` comments in the files to find all spots that need customization.

## Full documentation

- `docs/setup.md` — complete installation guide
- `docs/customization.md` — how to personalize each skill
- `config/scheduled-tasks.md` — cron job templates for all skills

## Repository structure

```
cowork-executive-assistant/
├── README.md
├── skills/
│   ├── _shared/
│   │   └── style-guide.md          ← your writing profile (template)
│   ├── meeting-prep/
│   │   └── SKILL.md
│   ├── action-items/
│   │   └── SKILL.md
│   ├── email-drafting/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── style-guide.md      ← redirect to _shared/
│   ├── executive-digest/
│   │   └── SKILL.md
│   └── humanizer/
│       └── SKILL.md
├── config/
│   └── scheduled-tasks.md
├── docs/
│   ├── setup.md
│   └── customization.md
└── LICENSE
```

## Contributing

PRs welcome. If you adapt a skill for a different language, tool, or workflow, please share.

## Credits

- Original concept: [mgonto/executive-assistant-skills](https://github.com/mgonto/executive-assistant-skills) for OpenClaw
- Adapted for Claude Cowork by Matheus Goyas (MG)

## License

MIT
