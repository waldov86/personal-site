---
title: "/ux-review"
description: "UI/UX review against accessibility, layout, typography, interaction, and data visualisation rules. Returns violations and top 3 recommendations ranked by user impact."
---

# /ux-review — UI/UX Review

Review the UI/UX of `$ARGUMENTS` using the ui-ux-pro-max design intelligence framework.

## Setup

Save this file to `~/.claude/commands/ux-review.md`.

Call it with:
```
/ux-review [screenshot, component name, or description]
```

---

## Instructions

Apply the full UI/UX Pro Max methodology:

### 1. Identify product type

Classify against the 161 product types and 10 BI/Analytics Dashboard archetypes (Data-Dense, Executive, Drill-Down Analytics, etc.)

### 2. Audit against the 10 priority rule categories

- **P1 Accessibility (CRITICAL):** contrast, focus states, ARIA, keyboard nav
- **P2 Touch & Interaction (CRITICAL):** target sizes, gesture conflicts
- **P3 Performance (HIGH):** lazy loading, virtualized lists, bundle
- **P4 Style Selection (HIGH):** visual consistency, dark mode, primary action
- **P5 Layout & Responsive (HIGH):** visual hierarchy, content priority, breakpoints
- **P6 Typography & Color (MEDIUM):** font pairing, semantic color, data readability
- **P7 Animation (MEDIUM):** duration 150-300ms, transform-only, interruptible
- **P8 Forms & Feedback (MEDIUM):** validation, loading/error/empty states
- **P9 Navigation Patterns (HIGH for dashboards):** IA hierarchy, back behavior, deep links, no mixed-level nav patterns
- **P10 Charts & Data (HIGH for dashboards):** chart-to-data-type match, tooltips, drill-down breadcrumbs, data density

### 3. Report format

- What's working well (with rule references)
- Violations (rule name + why it's a problem + concrete fix)
- Top 3 recommendations ranked by user impact

### 4. Pre-delivery checklist

Run through the visual quality, interaction, light/dark mode, layout, and accessibility checks.

If `$ARGUMENTS` is a screenshot or component description, focus there. If it's a general "dashboard" or system, review the overall IA and navigation patterns first.
