---
title: "The future of AI agents looks a lot like 2010."
description: "The thing missing from most agent setups isn't model quality or tool use. It's project management. Old patterns, new actors."
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

Three Markdown files and an Obsidian plugin. Board with three columns, each card a wikilink to a story file.

```markdown
## Backlog
- [ ] [[CVR-019-bedrock-card-rendering|CVR-019 Replace card rendering]]
- [ ] [[CVR-020-dashboard-rollout|CVR-020 Dashboard full rollout]]

## In Progress

## Done
- [x] [[PRJ-008-deploy-pipeline|PRJ-008 Deploy pipeline refactor]]
```

Each story file has a goal, context, acceptance criteria (hard gates and soft checks), an execution plan, a blocker field, and a work log. The hard gates are the definition of done — the story doesn't move until every one is checked. When something can't proceed, the card stays in Backlog marked `⚠️ BLOCKED`.

To start a session, I ask Claude to tackle the next story. Stories move to In Progress as the agent works — and come back to me when they need a decision or a human call. The board reflects who the ball is with.

Nothing gets worked on without a story. Creating one takes two minutes. It means Claude can pick up context in a new session without me re-explaining everything.

---

## The team

The team is also a skill — a Markdown file that defines roles, when to call them, and what each one is looking for. Not a flat list of tools. Named roles with specific jobs, called at specific points in the process.

The most useful one is [`/refinement-team-deep`](/skills/refinement-team-deep.md) — the planning council. Before any significant piece of work starts, I run the plan through six sequential agents — Critic, Contrarian, Tech Lead, Software Architect, Engineering Manager, then a Revised Planner who synthesises all five into a stronger output. Each agent addresses a flaw the previous one didn't. The Revised Planner can't ignore objections — it has to address each numbered finding or explicitly state why it's acceptable to leave it unresolved.

Concrete example: I was redesigning a campaign analysis pipeline — splitting it into two phases, one that classifies performance patterns, one that writes the actual analysis text. Before writing a line of code, I ran the plan through the council. The Contrarian flagged that shipping both phases together would make it impossible to isolate regressions if something broke. The Architect added that the phase boundary was wrong — the two parts were coupled in a way that would make the second phase significantly harder to execute.

The revised plan came back with a shadow mode gate: ship the classifier first, log its results without touching any visible output, verify it fires correctly on known cases, only then open the rendering story. A clear, testable definition of done that didn't require trusting the new logic was right — just that it was isolated.

The council costs about two minutes. It's saved me from bad architecture more than once.

The rest of the team follows the same pattern — a Markdown file, a defined lens, called at the right moment:

**Orchestrator (Claude as PM)** — reads the board, routes work, breaks epics into stories. Default mode.

**Tech Lead** — called before anything architectural ships. Reviews implementation for what will break in practice, not just what looks correct in theory.

**Designer** — I run `/ux-review` before writing a line of markup. Design review happens *before* implementation, not after.

**Critic** — `/critique` for a fast single pass when the planning council would be overkill. Returns a verdict (PROCEED / REVISE / ABORT) in under a minute.

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
