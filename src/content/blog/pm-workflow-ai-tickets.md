---
title: "Most AI-generated Jira tickets are useless. Here's the system that fixed mine."
description: "A set of Claude Code skills that encode your team's ticket philosophy once — so every Jira output starts grounded, not generic."
pubDate: 2026-04-09
tags: ["product-management", "claude-code", "jira", "workflow"]
hasMermaid: false
---

The tickets aren't the problem. The context is.

Every PM I know has tried asking an AI to write Jira tickets. Most got output that was technically coherent and practically useless — acceptance criteria that described a solution instead of a problem, padding dressed up as requirements, edge cases nobody asked for. The engineer reads it, asks three clarifying questions, and the ticket becomes a Slack thread anyway.

The failure isn't the model. It's that the model doesn't know your team's ticket philosophy, your product conventions, or what "done" means in your codebase. Generic input, generic output. Every time.

I built a different setup.

---

## What a good PM ticket actually does

Before the tooling: the mental model that changed how I write tickets.

A PM ticket is not a specification. It's a brief. Its job is to hand over *why* with enough clarity that the engineer can add their expertise — technical decisions, implementation tradeoffs, edge case handling — on top of a strong foundation, not on top of a vacuum.

The failure mode I kept seeing: tickets so prescriptive that the engineer's job became execution, not problem-solving. Or tickets so thin that they had to reconstruct the context themselves before they could even start. Both waste engineering capacity. Both produce worse outcomes than a well-framed brief would have.

The right ticket creates space. It gives the engineer something to build on — problem statement, user context, what good looks like — and leaves the *how* deliberately open for their judgment. That's not vagueness. That's respect for expertise.

AI-generated tickets fail here almost by default. A model trying to be helpful fills the space instead of preserving it.

---

## What I actually built

A set of Claude Code skills that encode the context *once* — ticket philosophy, product conventions, spec patterns, codebase architecture — so every invocation starts grounded, not generic.

The core principle: **alignment is human, but context persistence, scope refinement, and formatting are AI-native.** You write the brief. The system handles structure, completeness checks, and the right format for your team. You edit and own the output.

Three tools. One pipeline.

---

## `/jira` — the ticket skill you can steal right now

The philosophy is encoded directly into the prompt:

> Tickets are communication tools, not documentation dumps. Focus on the problem, not a prescribed solution. ACs must be testable and unambiguous. Leave room for technical judgment — the engineer's expertise belongs in the implementation, not the ticket. After drafting, trim aggressively — remove anything that doesn't help the person picking this up.

It infers ticket type from your input (story, task, bug, spike, epic), asks one clarifying question if the brief is too vague, drafts using the right template, then self-reviews before outputting — cutting anything added because it "might be useful."

The self-review step is the part most implementations skip. It's the part that makes the output usable.

The full skill file is on GitHub: [Jira: staying human among bots](https://gist.github.com/waldov86/62823ff6cc140416393b16e342b70b64). Copy it into your Claude Code commands folder as `Jira-skill.md`. Call it with `/jira user story: filter products by price range` and edit from there.

---

## `/prfaq` — more initiatives get a proper spec

A well-written PRFAQ creates alignment before significant resources are committed. The problem: writing one from scratch takes long enough that most PMs skip it for smaller initiatives. The bar becomes: is this big enough to justify the overhead?

The skill drafts a full PRFAQ in our Amazon-derived format — human-readable press release (one page, no template labels, no instructional artifacts), external FAQ anticipating customer questions, internal FAQ covering owner, resources, risks, and strategic fit. Unknowns are flagged `[Add: ...]` rather than hallucinated. A built-in self-review step checks that the press release reads like a human wrote it.

The gain isn't speed. It's that more initiatives get a proper PRFAQ, and the quality floor is higher.

---

## What makes it actually work

The context is encoded once. Product conventions, ticket philosophy, codebase architecture — every invocation builds on it. No re-explaining the same background. No generic output that needs heavy editing.

The human element stays central throughout. The PM owns the brief and the intent. The engineer owns the implementation and the technical judgment. AI handles the layer in between — structure, completeness, format — without collapsing the space where expertise lives.

Better PRFAQs create real alignment. Better tickets reduce rework. The gap between what product intended and what gets built closes — not because the ticket specified everything, but because it specified the right things.

---

**Stack:** Claude Code skills (Markdown prompt files) · Python backend (stage orchestration, quality scoring, state persistence) · shared context repo (product, architecture, team conventions) · Claude API via Amazon Bedrock
