# Applied AI Thinking — Skills

A small collection of Claude Code skills (`.skill` archives) packaged for personal use. Each `.skill` file is a zipped bundle containing a `SKILL.md` and any reference files; install by extracting into your skills directory or by importing through Claude Code.

## Skills in this folder

### 1. `brainstorm-companion.skill`

**What it does.** Turns Claude into a candid, PhD-level thinking partner for deep conversation, ideation, strategy stress-testing, and exploring tradeoffs. Treats the user as a domain expert, gives honest unsolicited judgments instead of agreeing by default, and aggressively grounds claims in current sources rather than relying on training-era knowledge. Stays in pure conversation mode — only produces mind maps or markdown captures when explicitly asked.

**Trigger phrases.** "let's think through", "help me explore", "play devil's advocate", "what are we missing", "let's workshop this", or any request to brainstorm/ideate/riff.

**Required tools / MCP connections.**
- **Web search** (built-in `WebSearch` / `WebFetch`) — used as a reflex throughout the conversation to ground claims and avoid stale knowledge. This is the only hard dependency.
- No external MCP servers required.

---

### 2. `daily-task-tracker.skill`

**What it does.** Scans your Fathom meeting recordings, Zoom messages, and Outlook emails for action items assigned to you, then creates and updates tasks in a Notion task database. Designed to be run as a periodic sweep so nothing falls through the cracks. On first run it auto-bootstraps: pulls your identity from Microsoft 365, optionally creates a Notion workspace for you, and saves your work-stream and channel preferences to `memory/task_tracker_config.md`.

**Trigger phrases.** "scan for new tasks", "update my task tracker", "what do I need to do", "run task tracker", "find action items", "sweep my emails/meetings/Zoom".

**Required MCP connections.**
- **Fathom** (`claude_ai_Fathom`) — `list_meetings`, `get_meeting_summary`, `get_meeting_transcript` for pulling action items out of meeting recordings.
- **Microsoft 365** (`claude_ai_Microsoft_365`) — `read_resource` (to auto-fetch the user's identity) and `outlook_email_search` for sweeping inbox threads.
- **Zoom Chat** (`zoom-chat`) — `scan_direct_messages`, `get_channel_messages`, `list_channels`, `analyze_message_relevance` for surfacing @mentions and pending threads.
- **Notion** (`claude_ai_Notion`) — `notion-create-pages`, `notion-create-database`, `notion-fetch`, `notion-query-database-view`, `notion-update-page` for managing the task and run-log databases.

All four MCP servers must be connected for the full sweep. The skill degrades gracefully if one source is unavailable, but a complete run depends on all of them.

---

### 3. `refine-prompts.skill`

**What it does.** Refines an existing prompt or drafts a new one from a described goal, applying the 2026 prompt-engineering best practices for Claude Opus 4.7, Sonnet 4.6, and Haiku 4.5. Output is a clean, copy-paste-ready prompt in a fenced code block — no preamble, no change log, no commentary unless explicitly requested. Includes reference files (`checklist.md`, `patterns.md`, `antipatterns.md`) covering structural patterns, common failure modes, and concrete examples for extraction/classification/generation/agentic tasks.

**Trigger phrases.** "refine", "improve", "rewrite", "fix", "polish", "level up" a prompt; "write a prompt for…", "prompt engineering", "make a better system prompt", or pasting a prompt with frustration that it isn't working.

**Required tools / MCP connections.**
- None. This skill is self-contained — it operates purely on the prompt text the user provides and its bundled reference files.

---

## Installing a skill

These are zipped skill bundles. To use one in Claude Code:

```bash
# Extract into your user skills directory
unzip brainstorm-companion.skill -d ~/.claude/skills/
```

Then invoke it by name, or let Claude trigger it automatically based on the description in `SKILL.md`.

## Quick reference — MCP requirements at a glance

| Skill | Web search | Fathom | Microsoft 365 | Zoom Chat | Notion |
|---|---|---|---|---|---|
| brainstorm-companion | required | — | — | — | — |
| daily-task-tracker | — | required | required | required | required |
| refine-prompts | — | — | — | — | — |
