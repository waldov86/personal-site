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

Three Markdown files and an Obsidian plugin.

**Board file** — the Obsidian Kanban plugin renders this as a board. Three columns: `Backlog`, `In Progress`, `Done`. Each card is a wikilink to a story file. Nothing else.

```markdown
## Backlog
- [ ] [[CVR-019-bedrock-card-rendering|CVR-019 Replace card rendering]]
- [ ] [[CVR-020-dashboard-rollout|CVR-020 Dashboard full rollout]]

## In Progress

## Done
- [x] [[PRJ-008-deploy-pipeline|PRJ-008 Deploy pipeline refactor]]
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

Hard gates are the definition of done — the story doesn't move until every one is checked. When something can't proceed, the card stays in Backlog marked `⚠️ BLOCKED`. The agent doesn't silently skip or abandon — it marks, explains, and stops.

I ask Claude to tackle the next story. Stories move to In Progress as the agent works — and come back to me when they need a human call. The board can't lie if Claude is the only thing that moves cards.

Before marking a story Done, there's a context update step. The work log and the project's `CLAUDE.md` are the place to capture what was decided and why — not just what was built. Zero checked boxes is not valid. Durable learnings don't survive on vibes.

Nothing gets worked on without a story.

---

## The team

The team is a Markdown file. Named roles, called at specific points in the process.

The most useful one is [`/refinement-team-deep`](/skills/refinement-team-deep.md) — the planning council. Six sequential agents: Critic, Contrarian, Tech Lead, Software Architect, Engineering Manager, then a Revised Planner who synthesises all five. Each addresses a flaw the previous one didn't.

Concrete example: I was redesigning a campaign analysis pipeline. The council flagged that shipping the new classification layer and the rendering change together would make it impossible to isolate regressions. The revised plan split them into separate stories — ship the classifier first in shadow mode, verify it on known cases, only then open the rendering work.

Two minutes of latency. The output was two new stories instead of one, with clearer hard gates on each.

**Orchestrator** — reads the board, routes work, breaks epics into stories. Default mode.

**Tech Lead** — called before anything architectural ships.

**Designer** — `/ux-review` before writing a line of markup.

**Critic** — `/critique` for a fast single pass. Returns PROCEED / REVISE / ABORT in under a minute.

---

## Where this is going

The thing that strikes me most about this setup is how familiar it feels. Backlog, stories, acceptance criteria, roles, reviews. We've been doing this for thirty years. AI agents didn't invent a new way of working — they just gave us a new team to manage.

That's the bet I'm making: the patterns from SDLC transfer, and the closer we follow them, the better agent-driven development gets. The overhead disappears. The discipline doesn't.

The open problems are real though. Context doesn't fully survive session boundaries — I've patched this with memory files and a weekly review skill that rebuilds state, but it's not elegant. Decision rationale evaporates from transcripts. Shared context across sessions and collaborators is still unsolved. [At AI Bar Camp Berlin last week, every room circled back to the same questions](https://waldo.vanderlore.de/blog/ai-camp-berlin-2026/). Nobody has cracked it.

My guess is that the answers look a lot like the questions — better story handoffs, better session summaries, better ways to make the work log the source of truth. Not new paradigms. Deeper investment in the old ones. But I'm genuinely not sure.

The kanban board is embarrassingly low-tech. That's the point.
