---
title: "I run my AI agents like an engineering team. It turns out SDLC is why."
description: "After a year of building with agents, the thing that actually works is embarrassingly familiar: kanban boards, tech leads, design reviews. Old patterns, new actors."
pubDate: 2026-04-28
tags: ["ai", "agents", "workflow", "claude-code", "engineering"]
hasMermaid: false
---

The most useful thing I've done for my agent setup wasn't a new model or a better prompt.

It was adding a kanban board.

---

## The thing nobody says out loud

There's a lot of talk about agent orchestration — tool use, memory layers, multi-agent routing. Almost none about how you actually manage the work.

I had the same gap. I'd open a Claude Code session, pick something to build, and let the agent run. Sometimes it worked. Often it drifted. Context would bleed between sessions. I'd come back three days later and have no idea what state things were in.

The problem wasn't the agent. It was that I'd stripped away every management structure from the workflow and wondered why nothing felt organized.

---

## What I now run

Three components. All of them familiar if you've worked in any engineering team.

**A kanban board.** Stories live in Backlog, In Progress, or Done. Each story is a `.md` file with an acceptance criterion — not a spec, a definition of done. When I have a new idea or stakeholder request, it goes to Backlog first. Nothing gets worked on without a story. The agent doesn't get to pick what's next.

**A team layout.** Not a flat list of tools — a set of named roles with specific responsibilities. The team I'm running includes:

- **Claude (PM + orchestrator)** — owns routing, manages the board, breaks down epics
- **Tech Lead** — reviews implementation before it ships, flags architectural decisions
- **Designer** — [ux-ui-implement skill](https://gist.github.com/waldov86/PLACEHOLDER_GIST_ID) — called before any UI work lands, not after
- **Critic / Contrarian** — adversarial review before significant decisions; runs as `/critique` for fast checks, `/refinement-team-deep` for planning council

**A code review step.** Not every change — but anything touching a critical path goes through Tech Lead review before it's marked done. This catches drift before it compounds.

The full team spec is here: [agent-team-setup.md](https://gist.github.com/waldov86/PLACEHOLDER_GIST_ID). Copy and adapt it. The roles that matter most in your setup will depend on what you're building.

---

## Why SDLC

When I first set this up, it felt slightly embarrassing. These are the same patterns we've been running for thirty years in software teams. I'm a PM-turned-builder playing pretend-manager with AI agents.

But the patterns exist because they solve real problems. Backlog grooming prevents work-in-progress explosion. Code review prevents quality drift. Named roles prevent diffuse accountability. None of that stops being true because the actors are artificial.

What's different: the cost of process is near zero. Adding a `/critique` call before a decision doesn't require a calendar invite. Having a Designer role doesn't require a headcount. The friction that makes process feel heavyweight in human teams mostly disappears.

So the patterns survive the transition, but shedding the overhead.

---

## What died

Prompt engineering as team design.

Tools like Kai and most multi-agent orchestration frameworks from 2024 treated "agent teams" as a collection of system prompts — give each agent a persona and a role description and watch them collaborate. I tried this. It felt clever for about a week.

The problem: everything lived in the prompt. The team's knowledge, its conventions, its history — all of it reset every session. You weren't building a team. You were re-instantiating a team from scratch every time you opened a terminal.

The direction that actually works: agents operating *within* an engineering system — version-controlled context, persistent state, defined process. The agent doesn't need to know it's a "Senior Tech Lead". It needs to receive the right context, in the right format, at the right time.

Standard templates help here. My skills are Markdown files in `~/.claude/commands/`. They're readable to humans, diffable in git, and composable. The `/refinement-team-deep` command runs a six-agent planning council — Critic, Contrarian, Tech Lead, Architect, EM, Synthesizer — not because each agent has a "persona", but because each step enforces a specific analytical lens that the previous one didn't.

The intelligence is in the structure, not the role label.

---

## The open problem

Context doesn't fully survive session boundaries. I've patched this with persistent memory files and a weekly review skill that rebuilds state. It works. It's not elegant.

The deeper issue: the agent and I make architectural decisions together in a session, and the record of that reasoning mostly lives in the conversation transcript. If that session ends and doesn't get summarized, the decision is orphaned from its rationale.

I haven't solved this. Neither has anyone else I've talked to. The best approach I've found: treat every significant decision as worth a one-sentence commit message — not to the codebase, but to the project's `CLAUDE.md`. It's slow. It's the only thing that works.

---

At AI Bar Camp Berlin last week, [every room circled back to the same problem](https://waldo.vanderlore.de/blog/ai-camp-berlin-2026/): shared context, shared process, legibility across people and sessions. Nobody has cracked it.

What I have is a team that ships. Not elegantly. But consistently.

The kanban board is embarrassingly low-tech. It's also the most important thing in the setup.
