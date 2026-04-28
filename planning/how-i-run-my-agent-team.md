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

**Story files** (`CVR-NNN-slug.md`) — one per card. Here's the actual schema:

```
Status / Type / Priority / Scope / Created / Started / Completed
Goal — one sentence
Context — why this exists, what problem it solves
Acceptance Criteria
  Hard gates: binary pass/fail conditions
  Soft checks: expected-true flags, worth noting if not
Pipeline phases — which pipeline steps are in scope
Execution plan
Blocker — type / description / unblock condition
Context update checklist — what docs to update after
Work log — timestamped entries per session
```

The acceptance criteria are the key piece. Not a spec — a definition of done. Hard gates are binary: the story doesn't move to Done unless every one is checked. Soft checks are flags: true in normal circumstances, worth noting when not. The agent knows it's done when all hard gates are checkable.

The blocker field is load-bearing. When a story can't proceed, the card stays in Backlog with `⚠️ BLOCKED` appended to its title. The blocker field records what's wrong and what would resolve it. The agent doesn't silently skip or abandon — it marks, explains, and stops.

**Pickup command** (`/cvr-next-story`) — a Claude Code skill that reads the board, picks the top Backlog card, reads the story file, and executes end-to-end. Before picking the next story, it checks whether something is stranded in In Progress. If it finds a card there with status DONE in the story file, it closes it out and continues. If it finds one still IN PROGRESS, it surfaces the three options: resume, mark blocked, or abort. The board can't lie to you if the skill is the only thing that moves cards.

After execution, the skill enforces a context update step before marking Done. The story file has a checklist of docs to update — `CLAUDE.md`, `docs/scripts.md`, whatever's relevant. Zero checked boxes is not valid. The skill won't mark Done without at least one checkbox checked or a one-sentence explanation of why nothing was worth capturing. Durable learnings don't survive on vibes.

The rule is simple: nothing gets worked on without a story. Stakeholder requests, ideas, things I notice in passing — Backlog first. The agent doesn't get to pick what's next based on what's most interesting.

This sounds bureaucratic. It isn't. Creating a story file takes two minutes. Having it means the agent can pick up context in a new session without me re-explaining everything.

---

## The team

Not a flat list of tools. Named roles with specific jobs, called at specific points in the process.

**Orchestrator (Claude as PM)** — reads the board, routes work, breaks epics into stories. Default mode.

**Tech Lead** — called before anything architectural ships. Reviews implementation for what will break in practice, not just what looks correct in theory. This is a skill invocation, not a separate agent — same model, different instruction set enforcing a different analytical lens.

**Designer** — called before any UI work starts. Runs design review against a library of patterns, accessibility rules, and component conventions. The discipline is: design review happens *before* implementation, not after.

**Critic / Contrarian** — adversarial review on significant decisions. Two tiers: `/critique` for fast single-pass (fatal flaws + verdict in under a minute), `/refinement-team-deep` for full planning council.

The planning council is the most useful thing I've built. It runs a plan through six sequential agents — Critic, Contrarian, Tech Lead, Software Architect, Engineering Manager, then a Revised Planner who synthesises all five into a stronger output. Each agent addresses a flaw the previous one didn't. The Revised Planner can't ignore objections — it has to address each numbered finding or explicitly state why it's acceptable to leave it unresolved.

Concrete example: I was planning a v2 architecture for an analysis pipeline — reclassifying campaign performance via a new `CampaignVerdict` dataclass, Python handling classification, Bedrock handling rendering. Before writing a line of code, I ran the plan through the council. The Contrarian flagged that the plan shipped a new classification layer and a rendering change simultaneously, making it impossible to isolate regressions. The Architect added that the phase boundary was wrong — classification and rendering were coupled in a way that would make the next phase significantly harder to execute.

The revised plan came back with a shadow mode gate: ship the classifier first, log verdicts per campaign without touching any analyst-visible output, run end-to-end on all advertisers, verify the classifier fires correctly on known cases, only then open the rendering story. That became CVR-018. The shadow mode approach also meant I could write precise hard gates: pipeline runs clean, no regressions, `_render_recommendation` output unchanged. A clear, testable definition of done that didn't require trusting that the new logic was right — just that it was isolated.

The council costs about two minutes of latency. It's saved me from bad architecture more than once.

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

My skills are Markdown files in `~/.claude/commands/`. The `/refinement-team-deep` skill is 142 lines. It runs six sequential analytical passes, enforces output formats for each, and requires the final planner to address every objection or explicitly justify ignoring it. None of that is intelligence. It's structure. The model provides the intelligence; the file provides the discipline. Readable to humans, diffable in git, composable with other skills. The role label is irrelevant — the instruction set is what creates the behaviour.

---

## The open problem

Context doesn't fully survive session boundaries. I've patched this with persistent memory files and a weekly review skill that rebuilds state. It works. It's not elegant.

The deeper issue: the agent and I make architectural decisions together in a session, and the record of that reasoning mostly lives in the conversation transcript. If the session ends without a summary, the decision is orphaned from its rationale. Three weeks later, when a story touches the same area, there's no way to recover *why* the earlier decision was made — only what it was.

I've built two partial mitigations. The first is the context update checklist in each story file — before any story moves to Done, the agent has to check off which docs it updated, or justify why it didn't. This forces decision rationale into `docs/scripts.md`, `docs/kpi-definitions.md`, or the project `CLAUDE.md` before the session closes. The second is `CLAUDE.md` as a standing instructions file: not a README, not code comments, but a file the agent reads at the start of every session containing the decisions that can't be inferred from the code. One-sentence entries like "the CVR reliable threshold is ≥20 conversions L14D — this was validated against the full advertiser set in April 2026 and is not arbitrary." That's the kind of thing that evaporates from transcripts.

It's still slow. It's still the only thing that works consistently. The goal is to make the standing instructions file the source of truth for anything a future-session agent would otherwise have to re-derive or re-ask.

What I have is a team that ships. Not elegantly. But consistently.

The kanban board is embarrassingly low-tech. It's also the most important thing in the setup.
