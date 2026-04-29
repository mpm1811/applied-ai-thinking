# Applied AI Thinking

> A small, curated set of **Claude Code skills** that turn Claude into a sharper thinking partner, an assistant that keeps your task list in sync with your meetings & inbox, and a prompt engineer that follows the 2026 best-practices.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-skills-7c5cff)](https://claude.com/claude-code)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)
[![Skills](https://img.shields.io/badge/skills-3-blue)](#skills)

---

## What is this?

Three drop-in [Claude Code skills](https://claude.com/claude-code) packaged as `.skill` archives. Each one is a single-file zipped bundle (`SKILL.md` + optional reference files) that Claude Code auto-loads from your skills directory and triggers based on the user's intent.

**Why these three?** They cover the three modes of work I reach for daily:

| Mode | Skill | One-liner |
|---|---|---|
| 🧠 **Think** | [`brainstorm-companion`](#-brainstorm-companion) | A PhD-level peer that argues with you, not a yes-machine |
| ✅ **Do** | [`daily-task-tracker`](#-daily-task-tracker) | Sweeps meetings + chats + email into a Notion task list |
| ✍️ **Write** | [`refine-prompts`](#-refine-prompts) | Refines any prompt to current Claude best-practices |

---

## Quick install

Pick the path that matches where you use Claude. The same `.skill` bundle works in both — only the install location differs.

### Option A — Claude.ai (web & desktop apps)

The fastest path. Upload the `.skill` file directly through the Claude UI; no shell or filesystem access needed.

1. Download the `.skill` archive(s) you want from the [Releases](https://github.com/mpm1811/applied-ai-thinking/releases) page or the repo root (right-click → "Save link as…").
2. Open Claude → **Settings** → **Capabilities** → **Skills**.
3. Click **Upload skill** and select the `.skill` file. Repeat for each one you want.
4. Toggle the skill on. It will auto-trigger from matching prompts.

> **Note for daily-task-tracker** — Claude.ai is where this skill shines, because the Fathom, Microsoft 365, Zoom Chat, and Notion MCP connectors live in Claude's settings. Connect those four under **Settings → Connectors** before running the skill.

### Option B — Claude Code (CLI / IDE)

```bash
# Clone or download
git clone https://github.com/mpm1811/applied-ai-thinking.git
cd applied-ai-thinking

# Extract whichever skills you want into your Claude Code skills directory
unzip brainstorm-companion.skill   -d ~/.claude/skills/
unzip daily-task-tracker.skill     -d ~/.claude/skills/
unzip refine-prompts.skill         -d ~/.claude/skills/
```

Restart Claude Code. The skills will auto-trigger on matching prompts, or invoke them explicitly with `/brainstorm-companion`, `/daily-task-tracker`, `/refine-prompts`.

> **Project-scoped install (Claude Code only)** — drop the extracted skill folder into `<project>/.claude/skills/` instead of `~/.claude/skills/` to scope it to a single project.

### Option C — Anthropic API / Agent SDK

If you're invoking Claude via the API, mount the extracted skill folder at the path you pass to the [Agent Skills](https://docs.anthropic.com/en/docs/claude-code/skills) loader. The `.skill` archive is just a zipped folder — extract it and point the SDK at the resulting directory.

---

## Skills

### 🧠 brainstorm-companion

Turns Claude into a candid, PhD-level thinking partner. Treats you as a domain expert, gives honest unsolicited judgment instead of agreeing by default, and uses web search reflexively to ground claims in current sources rather than training-era knowledge. Stays in conversation mode — no auto-generated mind maps unless you ask.

**Use it when** you say things like *"let's think through…", "play devil's advocate", "what are we missing", "let's workshop this"*.

**Example interaction**
```
You: I'm thinking about pivoting our pricing from per-seat to usage-based.
Claude: Before I weigh in — what behavior are you trying to change with the pivot?
        Existing customers expanding seats they don't use? Or new customers
        balking at the entry price? Those lead to different pricing shapes.
        [searches: usage-based pricing transitions B2B SaaS 2025]
        Two specific failure patterns I'd want you to defend against...
```

**Required tools**
- Built-in `WebSearch` / `WebFetch`. No external MCP needed.

---

### ✅ daily-task-tracker

Scans your Fathom meeting recordings, Zoom messages, and Outlook emails for action items addressed to you, then creates and updates tasks in a Notion database. Designed as a periodic sweep so nothing falls through the cracks. First run auto-bootstraps: pulls your identity from Microsoft 365, optionally creates a Notion workspace, and saves your work-stream preferences.

**Use it when** you say *"scan for new tasks", "what do I need to do", "run task tracker", "sweep my inbox"*.

**Example interaction**
```
You: run task tracker
Claude: Scanning for new items since 2026-04-28 09:14…
        ✓ Fathom: 3 meetings reviewed → 2 action items for you
        ✓ Outlook: 47 emails scanned → 1 new task, 2 updates
        ✓ Zoom: 4 channels + DMs → 1 thread needs your reply

        Created TSK-014: "Reply to vendor SLA proposal"  (High)
        Updated TSK-009: status → Blocked (waiting on legal)
```

**Required MCP connections** — all four needed for a full sweep:
- **Fathom** — `list_meetings`, `get_meeting_summary`, `get_meeting_transcript`
- **Microsoft 365** — `read_resource`, `outlook_email_search`
- **Zoom Chat** — `scan_direct_messages`, `get_channel_messages`, `list_channels`, `analyze_message_relevance`
- **Notion** — `notion-create-pages`, `notion-create-database`, `notion-fetch`, `notion-query-database-view`, `notion-update-page`

The skill degrades gracefully if one source is unavailable.

---

### ✍️ refine-prompts

Refines an existing prompt or drafts one from a described goal, applying the 2026 prompt-engineering best practices for Claude Opus 4.7, Sonnet 4.6, and Haiku 4.5. Default output is a clean, copy-paste-ready prompt in a fenced code block — no preamble, no change log. Bundled reference files cover structural patterns, common failure modes, and worked examples.

**Use it when** you say *"refine this prompt", "improve this", "write a prompt that…", "rewrite this so it actually works"*.

**Example interaction**
````
You: refine this — "summarize this article and tell me the main points"

Claude:
<task>
Summarize the article below and identify its main points.
</task>

<article>
{{ARTICLE_TEXT}}
</article>

<instructions>
Produce two sections:
1. A 3-4 sentence summary that captures the article's overall argument.
2. A numbered list of distinct main points, one sentence each.

Quote a short phrase (under 10 words) when a point relies on specific wording.
If the article is too short to identify distinct points, say so rather than inventing them.
</instructions>
````

**Required tools**
- None. Self-contained.

---

## MCP requirements at a glance

| Skill | Web search | Fathom | Microsoft 365 | Zoom Chat | Notion |
|---|:---:|:---:|:---:|:---:|:---:|
| brainstorm-companion | ✅ | — | — | — | — |
| daily-task-tracker | — | ✅ | ✅ | ✅ | ✅ |
| refine-prompts | — | — | — | — | — |

---

## Repository layout

```
applied-ai-thinking/
├── README.md
├── LICENSE                          # MIT
├── brainstorm-companion.skill       # zipped SKILL bundle
├── daily-task-tracker.skill         # zipped SKILL bundle
└── refine-prompts.skill             # zipped SKILL bundle (with references/)
```

Each `.skill` file is just a renamed `.zip`. To inspect a bundle without installing it:

```bash
unzip -l brainstorm-companion.skill
unzip -p brainstorm-companion.skill brainstorm-companion/SKILL.md | less
```

---

## Contributing

PRs and issues welcome — see the [good first issues](https://github.com/mpm1811/applied-ai-thinking/labels/good%20first%20issue) for places to start.

If you want to add a new skill:
1. Author it as a folder containing `SKILL.md` (and any `references/`).
2. Test it locally by symlinking into `~/.claude/skills/`.
3. Zip it: `cd your-skill && zip -r ../your-skill.skill .`
4. Open a PR with the `.skill` archive and a short description in the README.

For changes to existing skills, edit the contents of the bundle then repackage:

```bash
unzip refine-prompts.skill -d /tmp/refine
# edit /tmp/refine/refine-prompts/SKILL.md
cd /tmp/refine && zip -r refine-prompts.skill refine-prompts/
mv refine-prompts.skill /path/to/this/repo/
```

---

## License

[MIT](LICENSE) — use it, fork it, ship it.
