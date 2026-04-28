---
title: "We can each build anything. So why can't we build together?"
description: "Notes from AI Bar Camp Berlin 2026 — my talk, what the room taught me, and the shared context problem nobody has solved yet."
pubDate: 2026-04-28
tags: ["ai", "engineering", "teams", "agents", "reflection"]
---

I gave a talk at AI Bar Camp Berlin last week. The title was a question: *We can each build anything. So why can't we build together?*

It turned into the most useful conversation I've had about AI and software in a while — not because I had answers, but because everyone in the room was stuck on the same thing.

---

## The framework I opened with

To set the stage, I borrowed a framework from [Nate B. Jones](https://natesnewsletter.substack.com/p/the-5-level-framework-that-explains): five levels of AI integration in software engineering.

**L1 — Autocomplete.** AI fills in the gaps as you type. You still write all the logic.

**L2 — Chat-Assisted.** You describe what you want, the AI generates it. No execution, no verification — just output.

**L3 — Agentic.** AI reads files, navigates the codebase, makes multi-step changes. You're still in the loop, reviewing everything, but the agent is doing the heavy lifting.

**L4 — Harness-Driven.** AI writes, runs tests, reads failures, patches errors — a closed feedback loop. You define inputs, outputs, and acceptance criteria. The agent drives.

**L5 — Dark Factory.** Fully autonomous. You write specs and maintain verification systems. Everything else is the agent's job.

Most practitioners are somewhere between L2 and L3. A few are pushing into L4. L5 is still mostly theoretical.

The core insight is this: **getting past L3 isn't a tooling problem — it's a systems problem.** The bottleneck shifts from *how fast can I build this?* to *how well can I specify what I want, and how do I know it's actually done?*

---

## My own journey

I shared where I've been on this ladder:

**L2:** Invoice automation — drop a PDF in a folder, it lands in the right row in a tax spreadsheet. This worked cleanly. Narrow problem, clear output, easy to verify.

**L3:** A campaign analysis pipeline — 181 ad campaigns, run headless every night, with automated QA gates. I'm squarely here. Claude Code is my interface. Most of my personal projects live in this zone.

**L3 → L4:** I'm figuring this out. The tooling is available. The gap is specification quality and knowing when to trust the loop.

---

## The solo builder trap

Here's what I hit that I suspect many people in that room also hit.

Solo, I move fast. Very fast. But that velocity comes at a cost: **the context lives in my head, spread across fifty agent sessions, and nowhere else.**

When I try to bring a second person in, we don't move twice as fast — we move backwards for a week while I try to transfer context that was never written down because the agent and I figured it out together.

Three things are broken that used to just be inconvenient:

- **Context is unshared.** Nobody else knows why the code does what it does. The agent knew, but it's gone.
- **Review is broken.** How do you review a PR where the agent wrote 80% of it? You're reviewing the output of a conversation you weren't in.
- **Velocity doesn't transfer.** The second person joins and you're back to meetings, because the speed was personal, not structural.

---

## The L4 mirage

Consulting with the Faust AI team while preparing this talk, I saw something building at L4 — agents in production, a memory layer, self-optimising prompts. Genuinely impressive. But when I looked closely, there was a pattern that concerned me.

Only one engineer understood the full system. There was no closed feedback loop with humans outside that engineer. Remove that person and everything stops — because *the engineer IS the harness.*

That's not L4. That's a more fragile version of L3, with better aesthetics.

Real L4 needs to be legible to someone other than its author. That's the part nobody has solved.

---

## The same problem, in every room

My session was in the morning. I spent the rest of the day walking into other rooms — agent orchestration, knowledge management, team velocity, tooling — and the same challenge kept surfacing. Different framing, different industry, different stack. Same underlying thing.

**It's all about shared context, shared knowledge, shared data.**

Some people think the answer is git-based — every piece of context lives in version-controlled files that agents and humans read from the same place. Others believe in vertical context managers — humans (often PMs, actually) who own the canonical understanding of a domain and promote it as best practice.

One framing that kept coming up: *product managers are becoming context managers.* The job isn't just defining what to build — it's maintaining the shared truth that agents can act on. The question is whether that truth is a single evolving document (Karpathy-style), or a constellation of files in disparate places that together constitute reality.

Nobody had a clean answer.

What did feel settled across the rooms:

- Claude is the dominant tool (the echo was loud and unprompted)
- We're re-inventing SDLC patterns for agents — kanban boards, tech lead roles, design reviews — because the old patterns still make sense, even if the actors have changed
- Prompt-engineering-based team setups feel like 2024 thinking; the direction is toward agents operating within proper engineering systems, not instructions masquerading as architecture
- **Decisions are the bottleneck.** Not code generation, not infrastructure. The rate at which humans make good decisions is now the limiting factor.

---

## What's still unsolved

I asked the room directly: has anyone cracked the onboarding problem? The context-sharing problem? Code review at L4?

Nobody has. A few had partial approaches. Most had workarounds.

The questions I'm still sitting with:

- How do you review code when the agent wrote most of it and the conversation that generated it is gone?
- How do you onboard a new engineer into an AI-native codebase where half the architectural decisions exist only in agent context?
- How do you maintain ownership when agents can touch anything?

If you've got something working, I genuinely want to hear it.

---

## The slides

If you want to go deeper on any of this, the live slides from my talk are here: [waldo.vanderlore.de/talks/aicamp-2026/](https://waldo.vanderlore.de/talks/aicamp-2026/)

The deck is interactive — it's the actual version I presented.

---

AI Bar Camp Berlin was worth the day. Barcamp format means you get self-selected rooms of people who actually care about the thing — no vendor pitches, no polished narratives, just practitioners comparing notes. I'll be back next year, hopefully with fewer open questions and at least one solved one.
