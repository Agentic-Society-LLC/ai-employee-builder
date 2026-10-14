# ai-employee-builder

Hire your first AI employee. An interview-led skill from Agentic Society that turns a role on your org chart (AI CMO, AI Social Media Manager, AI Project Manager, AI CFO, AI Bookkeeper, AI Sales Coach, AI Client Success Manager, and more) into a named, personalized AI agent: a soul file, a job contract, routines that run on a schedule, triggers that react to events, connected tools, and a tiered go-live on **Muse, Grok, Dots, Hermes or OpenClaw**.

**Hire for the role, not the task.** You answer about thirty plain questions, one at a time. Your AI looks up what it can, proposes the rest, and shows you what you decided versus what it assumed before it writes anything. A progress page grows as you go.

## What you get

- `ai-employee/<name>/progress.html`: the tracker. Open it in a browser, turn on Auto-refresh, watch the bar fill: the employee card, the contract (DECIDED / ASSUMED), files, tools, routines, triggers, go-live steps, the autonomy ladder.
- The **employee packet**: `SOUL.md`, `IDENTITY.md`, `USER.md`, `AGENTS.md`, `TOOLS.md`, `ROUTINES.md`, `triggers.md`, one `skills/<job>.md` per recurring job, `MEMORY.md`, `docs/PRD.md`, `docs/runbook.md`, `docs/launch-gate.md`, and a one-page `README.md` card. Written in your company's context; the source of truth on every platform.
- A **platform recommendation** from your tech level (1-10) and what you intend to build: Muse / Grok / Dots for most owners, Hermes / OpenClaw when you mean to build on APIs, MCPs and CLIs. With a reason and a runner-up.
- **Go-live**, tiered: the simple tier ends when one routine has produced one output you reviewed; the advanced tier walks the nine gated stages (a skeleton run that leaves a record, the four floor controls, the failure tests, a mechanical launch gate, promotion on evidence). Both end on the earned-autonomy ladder.

Run it on **Claude Opus 5.5** if you can; it holds the whole hire in its head.

## Install

**One click:** [agenticsociety.com/downloads/ai-employee-builder-skill](https://agenticsociety.com/downloads/ai-employee-builder-skill) downloads the zip and shows the install for each tool.

**Claude Code** (paste into your terminal)
```bash
mkdir -p ~/.claude/skills && cd ~/.claude/skills && curl -sL https://github.com/Agentic-Society-LLC/ai-employee-builder/archive/refs/heads/main.tar.gz | tar xz && rm -rf ai-employee-builder && mv ai-employee-builder-main ai-employee-builder
```
Then type `/ai-employee-builder` in any session. Running it inside your company brain folder is best: it can read your voice docs and check what is already connected.

**Codex** (paste into your terminal)
```bash
mkdir -p ~/.codex/skills && cd ~/.codex/skills && curl -sL https://github.com/Agentic-Society-LLC/ai-employee-builder/archive/refs/heads/main.tar.gz | tar xz && rm -rf ai-employee-builder && mv ai-employee-builder-main ai-employee-builder
```
Restart Codex, then say "build my first AI employee".

**Claude.ai / Cowork**
Download the zip from the link above and upload it as-is in Settings → Customize → Skills. Then say "build my first AI employee".

**Hermes Agent**
```bash
mkdir -p ~/.hermes/skills/business && cd ~/.hermes/skills/business && curl -sL https://github.com/Agentic-Society-LLC/ai-employee-builder/archive/refs/heads/main.tar.gz | tar xz && rm -rf ai-employee-builder && mv ai-employee-builder-main ai-employee-builder
```

**OpenClaw**
```bash
cd ~/.openclaw/workspace/skills && curl -sL https://github.com/Agentic-Society-LLC/ai-employee-builder/archive/refs/heads/main.tar.gz | tar xz && rm -rf ai-employee-builder && mv ai-employee-builder-main ai-employee-builder
```

**Anything else**
Open [SKILL.md](https://raw.githubusercontent.com/Agentic-Society-LLC/ai-employee-builder/main/SKILL.md), copy all of it, paste it as your first message, then say "start". The references it points at are in this repo under `references/`; paste the one it asks for when it asks.

## How it runs

1. **Interview** (six phases, one question per message): you and the company → the role and its seat → the dream hire and its name → jobs, routines and triggers → tools and access → boundaries and platform.
2. **Playback**: the agent contract as a table, every row DECIDED or ASSUMED, plus the employee introducing itself in its own voice. Nothing is written until you confirm.
3. **Packet**: the files, in your company's context.
4. **Go-live**: the tier's steps, one per message, each ending with something you can check with your own eyes, including flipping the stop button yourself.
5. **Improve**: come back with "it did X wrong" or "make it better" and it captures the lesson, updates the job's skill file, and tells you whether the evidence supports the next rung of autonomy.

## The tracker

One self-contained HTML file. Your AI edits only the JSON block inside it (`<script id="employee-data">`); the page renders everything else. The template is [assets/progress.html](assets/progress.html) and ships with sample data until your AI personalizes it.

## Built on

The Agentic Society *Agent Builder Starter Prompt*, *Build Guide* and *Hackathon Deck* (agenticsociety.com/resources), and the eight AI employees Agentic Society runs in production. Every control in `references/go-live.md` exists because one of them broke first.

## License

MIT. Use it, adapt it, share it. See [LICENSE](LICENSE).
