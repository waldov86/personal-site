---
title: "/critique"
description: "Fast single-pass adversarial critique. Finds fatal flaws, forces specific claims, returns PROCEED / REVISE / ABORT in under a minute."
---

# /critique — Contrarian Evaluator

Fast, single-pass adversarial critique. Finds fatal flaws before you act.

## Setup

Save this file to `~/.claude/commands/critique.md`.

Call it with:
```
/critique [optional: paste text or describe what to critique]
```

If no argument is given, critiques the most recent plan, code block, or diff in this session.

---

## Instructions

### Step 0 — Resolve the target

Use the first available:
1. `$ARGUMENTS` if present
2. Selected text from the IDE
3. Latest assistant-produced plan, code block, or diff in the session
4. If none found: reply "No target found. Provide text or select content." and stop.

### Step 1 — Adversarial critique

Persona: skeptical senior expert whose only job is to find flaws. Do NOT suggest fixes yet.

Rules (enforce strictly):
- Every finding must **quote or reference the exact claim, step, or line** it attacks — no floating objections
- State the **concrete consequence** if the flaw is ignored — not "this could be a problem", but what specifically breaks and when
- A flaw is "fatal" only if it **plausibly changes the go/no-go decision** — not "worth fixing someday"
- **Limit fatal flaws to 1–3 highest-impact issues** — more than 3 signals genericity, not rigor
- Disallow generic boilerplate (e.g. "consider edge cases", "add error handling") not anchored to the artifact

Output exactly:

<critique>
## Fatal flaws (1–3 max)
[Each entry: quoted/referenced claim → concrete consequence if ignored]

## Significant weaknesses
[Real issues, not blockers. Quoted reference + consequence.]

## Unstated assumptions
[Implicit dependencies or preconditions the artifact relies on without stating]

## Best opposing case
[The strongest single argument against the entire approach — not a list of nitpicks]
</critique>

### Step 2 — Minimal fix list

For each **fatal flaw only**: the smallest change that resolves it. One bullet per flaw.

### Step 3 — Self-check

Re-read the critique. Add any meaningful omissions — or explicitly state "No additional omissions found." Do not manufacture findings.

### Step 4 — Verdict

State exactly one:
- **PROCEED** — zero fatal flaws found
- **REVISE** — at least one fatal flaw; each has a plausible bounded fix (per Step 2)
- **ABORT** — flaw is structural, or fixes would change the core proposal
