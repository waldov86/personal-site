---
title: "Scrum for AI agents. Zero complaints about estimation."
description: "The thing missing from most agent setups isn't model quality or tool use. It's project management. Old patterns, new actors — and finally a team that doesn't hate the tickets."
pubDate: 2026-04-28
tags: ["ai", "agents", "workflow", "claude-code", "engineering"]
hasMermaid: false
---

The most useful thing I've done for my agent setup wasn't a new model or a better prompt.

It was adding a kanban board.

---

## The problem with just letting the agent run

The conversation around AI agents is almost entirely about orchestration — tool use, memory layers, multi-agent routing. Almost none of it is about managing the work.

I had the same gap. I'd open a Claude Code session, pick something to build, and let the agent run. Sometimes it worked. Often it drifted. Context would bleed between sessions. I'd come back three days later with no idea what state things were in or why something had been built the way it was.

The problem wasn't the agent. It was that I'd stripped away every management structure from the workflow and expected nothing to change.

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
Pipeline phases — which steps are in scope
Execution plan
Blocker — type / description / unblock condition
Context update checklist — what docs to update after
Work log — timestamped entries per session
```

The acceptance criteria are the key piece. Not a spec — a definition of done. Hard gates are binary: the story doesn't move to Done unless every one is checked. Soft checks are flags: true in normal circumstances, worth noting when not. The agent knows it's done when all hard gates are checkable.

The blocker field is load-bearing. When a story can't proceed, the card stays in Backlog with `⚠️ BLOCKED` appended to its title. The blocker field records what's wrong and what would resolve it. The agent doesn't silently skip or abandon — it marks, explains, and stops.

To start a session, I ask Claude to tackle the next story. That's it. It reads the board, picks the top card, reads the story file, and we go. Before picking up something new, it checks whether anything is stranded in In Progress — closes it out if it's done, surfaces the options if it isn't. The board can't lie if Claude is the only thing that moves cards.

Before marking a story Done, there's a context update step. The story file has a checklist of docs to update — `CLAUDE.md`, `docs/scripts.md`, whatever's relevant. Zero checked boxes is not valid. Durable learnings don't survive on vibes.

The rule is simple: nothing gets worked on without a story. Stakeholder requests, ideas, things I notice in passing — Backlog first. The agent doesn't get to pick what's next based on what's most interesting.

This sounds bureaucratic. It isn't. Creating a story file takes two minutes. Having it means the agent can pick up context in a new session without me re-explaining everything.

---

## The team

Not a flat list of tools. Named roles with specific jobs, called at specific points in the process.

**Orchestrator (Claude as PM)** — reads the board, routes work, breaks epics into stories. Default mode.

**Tech Lead** — called before anything architectural ships. Reviews implementation for what will break in practice, not just what looks correct in theory. This is a skill invocation, not a separate agent — same model, different instruction set, different analytical lens.

**Designer** — called before any UI work starts. I run `/ux-review` before writing a line of markup — it audits against accessibility rules, component conventions, layout patterns, and catches the things that are cheap to fix before implementation and expensive after. The discipline is: design review happens *before* implementation, not after.

**Critic / Contrarian** — adversarial review on significant decisions. Two tiers: `/critique` for a fast single pass — it finds fatal flaws, forces you to quote the specific claim it's attacking, and returns a verdict (PROCEED / REVISE / ABORT) in under a minute. Then `/refinement-team-deep` for a full planning council when the stakes are higher.

The planning council is the most useful thing I've built. It runs a plan through six sequential agents — Critic, Contrarian, Tech Lead, Software Architect, Engineering Manager, then a Revised Planner who synthesises all five into a stronger output. Each agent addresses a flaw the previous one didn't. The Revised Planner can't ignore objections — it has to address each numbered finding or explicitly state why it's acceptable to leave it unresolved.

Concrete example: I was redesigning a campaign analysis pipeline — splitting it into two phases, one that classifies performance patterns, one that writes the actual analysis text. Before writing a line of code, I ran the plan through the council. The Contrarian flagged that shipping both phases together would make it impossible to isolate regressions if something broke. The Architect added that the phase boundary was wrong — the two parts were coupled in a way that would make the second phase significantly harder to execute.

The revised plan came back with a shadow mode gate: ship the classifier first, log its results per campaign without touching any visible output, verify it fires correctly on known cases, only then open the rendering story. The shadow mode approach also meant I could write precise hard gates — pipeline runs clean, no regressions, existing output unchanged. A clear, testable definition of done that didn't require trusting the new logic was right — just that it was isolated.

The council costs about two minutes of latency. It's saved me from bad architecture more than once.

The skill file is [here](/skills/refinement-team-deep.md) — drop it into `~/.claude/commands/` and call it with `/refinement-team-deep [plan text or file path]`.

---

## What died

Prompt engineering as team design.

Most multi-agent frameworks from 2024 treated "agent teams" as a collection of system prompts — give each agent a persona and a role description and watch them collaborate. I tried this. It felt clever for about a week.

The problem: everything lived in the prompt. The team's knowledge, its conventions, its history — all of it reset every session. You weren't building a team. You were re-instantiating one from scratch every time you opened a terminal.

The direction that actually works: agents operating *within* an engineering system — version-controlled context, persistent state, defined process. The agent doesn't need to know it's a "Senior Tech Lead". It needs to receive the right context, in the right format, at the right time.

My skills are Markdown files in `~/.claude/commands/`. The `/refinement-team-deep` skill is 142 lines. It runs six sequential analytical passes, enforces output formats for each, and requires the final planner to address every objection or explicitly justify ignoring it. None of that is intelligence. It's structure. The model provides the intelligence; the file provides the discipline. The role label is irrelevant — the instruction set is what creates the behaviour.

What's different: the cost of process is near zero. A `/critique` call doesn't require a calendar invite. A design review doesn't require a headcount. The friction that makes SDLC feel heavy in human teams mostly disappears. The patterns survive. The overhead doesn't.

---

## The open problem

Context doesn't fully survive session boundaries. I've patched this with persistent memory files and a weekly review skill that rebuilds state. It works. It's not elegant.

The deeper issue: the agent and I make architectural decisions together in a session, and the record of that reasoning mostly lives in the conversation transcript. If the session ends without a summary, the decision is orphaned from its rationale. Three weeks later, when a story touches the same area, there's no way to recover *why* the earlier decision was made — only what it was.

I've built two partial mitigations. The first is the context update checklist in each story file — before any story moves to Done, the agent has to check off which docs it updated, or justify why it didn't. This forces decision rationale into the relevant docs before the session closes. The second is `CLAUDE.md` as a standing instructions file: not a README, not code comments, but a file the agent reads at the start of every session containing decisions that can't be inferred from the code. One-sentence entries. That's the kind of thing that evaporates from transcripts.

It's still slow. It's still the only thing that works consistently.

---

At AI Bar Camp Berlin last week, [every room circled back to the same problem](https://waldo.vanderlore.de/blog/ai-camp-berlin-2026/): shared context, shared process, legibility across sessions and people. Nobody has cracked it.

What I have is a team that ships. Consistently, if not elegantly.

The kanban board is embarrassingly low-tech. That's the point. Process works because it's boring, not despite it.
