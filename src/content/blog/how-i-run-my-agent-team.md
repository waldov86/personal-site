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

## The problem with just letting the agent run

There's a lot of talk about agent orchestration — tool use, memory layers, multi-agent routing. Almost none about how you actually manage the work.

I had the same gap. I'd open a Claude Code session, pick something to build, and let the agent run. Sometimes it worked. Often it drifted. Context would bleed between sessions. I'd come back three days later and have no idea what state things were in or why something had been built the way it was.

The problem wasn't the agent. It was that I'd stripped away every management structure from the workflow and wondered why nothing felt organized.

---

## The kanban board

Here's the actual setup. It's three Markdown files and an Obsidian plugin.

**Board file** (`kanban-cvr-analyzer.md`) — the Obsidian Kanban plugin renders this as a board. Three columns: `Backlog`, `In Progress`, `Done`. Each card is a wikilink to a story file. Nothing else.

```markdown
## Backlog
- [ ] [[CVR-019-bedrock-card-rendering|CVR-019 Replace card rendering]]
- [ ] [[CVR-020-dashboard-rollout|CVR-020 Dashboard full rollout]]

## In Progress

## Done
- [x] [[CVR-014-hackathon-tooling|CVR-014 Adopt hackathon winner tooling]]
```

**Story files** (`CVR-NNN-slug.md`) — one per card. The format: status, type, goal, context, acceptance criteria (hard gates and soft checks), execution plan, blocker field, work log. The story file is the source of truth. The board is derived.

The acceptance criteria are the key piece. Not a spec — a definition of done. Hard gates are binary: either this is true or the story doesn't move to Done. Soft checks are flags: true in normal circumstances, worth noting if not. The agent knows it's done when all hard gates are checkable.

**Pickup command** (`/cvr-next-story`) — a Claude Code skill that reads the board, picks the top Backlog card, reads the story file, and executes end-to-end. It checks for stranded In Progress items first. It doesn't ask what to work on next. The board decides.

The rule is simple: nothing gets worked on without a story. Stakeholder requests, ideas, things I notice in passing — Backlog first. The agent doesn't get to pick what's next based on what's most interesting.

This sounds bureaucratic. It isn't. Creating a story file takes two minutes. Having it means the agent can pick up context in a new session without me re-explaining everything.

---

## The team

Not a flat list of tools. Named roles with specific jobs, called at specific points in the process.

**Orchestrator (Claude as PM)** — reads the board, routes work, breaks epics into stories. Default mode.

**Tech Lead** — called before anything architectural ships. Reviews implementation for what will break in practice, not just what looks correct in theory. This is a skill invocation, not a separate agent — same model, different instruction set enforcing a different analytical lens.

**Designer** — called before any UI work starts. Runs design review against a library of patterns, accessibility rules, and component conventions. The discipline is: design review happens *before* implementation, not after.

**Critic / Contrarian** — adversarial review on significant decisions. Two tiers: `/critique` for fast single-pass (fatal flaws + verdict in under a minute), `/refinement-team-deep` for full planning council.

The planning council is the most useful thing I've built. It runs a plan through six sequential agents — Critic, Contrarian, Tech Lead, Software Architect, Engineering Manager, then a Revised Planner who synthesizes all five into a stronger output. Each agent addresses a flaw the previous one didn't. The Revised Planner can't ignore objections — it has to address each numbered finding or explicitly state why it's acceptable to leave it unresolved.

The skill file is [here](/skills/refinement-team-deep.md) — drop it into `~/.claude/commands/` and call it with `/refinement-team-deep [plan text or file path]`.

---

## Why SDLC

When I first set this up, it felt slightly embarrassing. These are patterns we've been running for thirty years in software teams. I'm a PM-turned-builder playing pretend-manager with AI agents.

But the patterns exist because they solve real problems. Backlog grooming prevents work-in-progress explosion. Code review prevents quality drift. Named roles prevent diffuse accountability. None of that stops being true because the actors are artificial.

What's different: the cost of process is near zero. Adding a `/critique` call before a decision doesn't require a calendar invite. Running a design review doesn't require a headcount. The friction that makes process feel heavyweight in human teams mostly disappears.

So the patterns survive the transition, but shedding the overhead.

---

## What died

Prompt engineering as team design.

Most multi-agent frameworks from 2024 treated "agent teams" as a collection of system prompts — give each agent a persona and a role description and watch them collaborate. I tried this. It felt clever for about a week.

The problem: everything lived in the prompt. The team's knowledge, its conventions, its history — all of it reset every session. You weren't building a team. You were re-instantiating one from scratch every time you opened a terminal.

The direction that actually works: agents operating *within* an engineering system — version-controlled context, persistent state, defined process. The agent doesn't need to know it's a "Senior Tech Lead". It needs to receive the right context, in the right format, at the right time.

My skills are Markdown files in `~/.claude/commands/`. They're readable to humans, diffable in git, and composable. The intelligence is in the structure, not the role label.

---

## The open problem

Context doesn't fully survive session boundaries. I've patched this with persistent memory files and a weekly review skill that rebuilds state. It works. It's not elegant.

The deeper issue: the agent and I make architectural decisions together in a session, and the record of that reasoning mostly lives in the conversation transcript. If the session ends without a summary, the decision is orphaned from its rationale.

I haven't solved this. The best approach I've found: treat every significant decision as worth a one-sentence entry in the project's `CLAUDE.md`. Not the codebase — the standing instructions file the agent reads at the start of every session. It's slow. It's the only thing that works consistently.

---

At AI Bar Camp Berlin last week, [every room circled back to the same problem](https://waldo.vanderlore.de/blog/ai-camp-berlin-2026/): shared context, shared process, legibility across sessions and people. Nobody has cracked it.

What I have is a team that ships. Not elegantly. But consistently.

The kanban board is embarrassingly low-tech. It's also the most important thing in the setup.
