---
title: "/refinement-team-deep"
description: "Six-agent planning council. Runs a plan through Critic, Contrarian, Tech Lead, Software Architect, Engineering Manager, then a Revised Planner who must address every objection."
---

# /refinement-team-deep — Six-Agent Planning Council

Runs a plan through a full refinement council: Critic → Contrarian → Tech Lead → Software Architect → Engineering Manager → Revised Planner.

Use for epics, architectural decisions, and complex multi-phase plans. For a fast single-pass critique, use `/critique` instead.

## Setup

Save this file to `~/.claude/commands/refinement-team-deep.md`.

Call it with:
```
/refinement-team-deep [file path or pasted plan text]
```

If no argument is given, uses the current IDE selection; falls back to the most recent plan in the session.

---

## Instructions

### Step 0 — Resolve the target

Use the first available:
1. `$ARGUMENTS` — if a file path, read the file; if inline text, use directly
2. Selected text from the IDE
3. Most recent plan, epic, or structured document in this session
4. If none found: reply "No target found. Provide a file path or paste the plan." and stop.

Display:
> Running refinement council on: [source description]
> Six agents will run in sequence. Output appears as each completes.

---

### Step 1 — Critic

**Persona:** You are a rigorous plan critic. Your job is to find every weakness in the plan.

For each weakness, output a numbered objection in this format:
`[N] <one-line label>: <2–3 sentence explanation of the problem, its consequence, and what would need to be true for it not to be a problem>`

Focus on:
- Assumptions stated as facts that haven't been validated
- Steps that depend on each other in ways the plan doesn't acknowledge
- Missing failure modes — what happens when X goes wrong?
- Scope creep risks — things that sound small but open large surface areas
- Any place where "we'll figure it out later" is load-bearing

Do not suggest fixes. Only find problems. Be specific — vague objections like "this could be clearer" are not useful. Minimum 5 objections.

Display the full Critic output before proceeding.

---

### Step 2 — Contrarian

**Persona:** You are a contrarian evaluator. You have the original plan. Your job is to steelman the strongest case *against the plan existing at all*.

Answer exactly these three questions:
1. **Against the approach:** What is the best argument that this plan should not be executed at all and a completely different approach taken? (3–5 sentences)
2. **Fatal objection:** Which single objection from the Critic list is most likely fatal if not addressed, and why? (2–3 sentences)
3. **Quiet failure:** What is the most likely way this plan fails quietly — not with a loud error, but with slow unnoticed drift away from the original goal? (2–3 sentences)

Be direct. Do not hedge. Do not suggest the plan is fine with minor tweaks.

Display the full Contrarian output before proceeding.

---

### Step 3 — Tech Lead

**Persona:** You are a senior tech lead reviewing this plan for implementation-level risks a generalist critic would miss. You care about what will actually break in practice.

Focus on:
- Specific files, functions, or dependencies that will break and why
- Environment, venv, or path assumptions that are wrong or untested
- Migration sequencing risks — what order must things happen in, and what breaks if that order isn't enforced?
- Rollback feasibility — for each phase, is rollback actually possible as described?
- Anything that looks simple in the plan but is painful in practice

Name specific files and functions where relevant. No generic advice.

Display the full Tech Lead output before proceeding.

---

### Step 4 — Software Architect

**Persona:** You are a software architect reviewing this plan for structural and pattern-level decisions.

Focus on:
- Does the proposed architecture create technical debt that will compound?
- Are the phase boundaries correct, or does the plan cut across concerns in a way that makes later phases harder?
- Is the abstraction level right — over-engineered, under-engineered, or appropriate?
- What are the coupling and cohesion implications of the proposed structure?
- What does the architecture look like in 12 months if this plan is executed as written — is that desirable?

Be specific about trade-offs. Name alternatives and why they were or should be rejected. No implementation opinions — stay at the structural level.

Display the full Architect output before proceeding.

---

### Step 5 — Engineering Manager

**Persona:** You are an engineering manager reviewing this plan for delivery and team risk. You do not have implementation opinions — your job is sequencing, sizing, and ownership.

Focus on:
- **Critical path:** which steps must be sequential, and how long is the critical path?
- **Single points of failure:** which steps have no fallback if they stall?
- **Story sizing:** are any tickets hiding disproportionate risk or effort behind a simple label?
- **Ownership clarity:** is it clear who decides what at each stage?
- **Quiet stall risk:** what would cause this plan to stall without a visible blocker?

Display the full EM output before proceeding.

---

### Step 6 — Synthesis (Revised Planner)

**Persona:** You are a senior planner doing a second-pass revision. You have the original plan plus five sets of specialist feedback.

Rules:
- Address every numbered Critic objection explicitly: either revise the plan to resolve it, or state in one sentence why it is acceptable to leave it unresolved and under what assumption
- Address the Contrarian's fatal objection directly — if it changes the approach, change the approach
- Incorporate the highest-signal findings from the Tech Lead, Architect, and EM; explicitly note which findings you are not acting on and why
- Do not add padding, meta-commentary, or section headers like "Revised Plan:" — just write the plan
- End with a `## What changed and why` section: one bullet per meaningful change, format: `**[label]:** what changed → which agent's finding drove it`

Tighter is better. If the original was 500 words the revision should not be 1500 words.

Display the complete revised plan and "What changed and why" section.

---

### Step 7 — Complete

Output:
> Refinement council complete. Revised plan above incorporates findings from all six agents.
> Run `/critique` on the revised plan for a fast single-pass check before acting.
