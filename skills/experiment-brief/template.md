# Experiment brief template

Copy everything below the line. Keep the section order. Delete the guidance in *italics*.

---

```
---
title: "<Experiment name>"
team: "<team from setup>"
status: "<matches pipeline: draft | approved | running | readout | shipped | killed>"
date: "YYYY-MM-DD"
owner: "<owner from setup>"
theme: "<activation | retention | monetization | ...>"
experiment_flag: "TBD"
design_lead: "<from setup, or TBD>"
engineering_lead: "<from setup, or TBD>"
tags: [<team>, <theme>]
---
```

# <Experiment name>

> **TL;DR:** *One sentence: the change, the audience, the metric it should move.*

## Problem & evidence

*One or two sentences on the problem.*

| Evidence | Source |
|---|---|
| *Quote or specific behavior* | *source name* |
| **Counter-case, problem absent:** *…or "none found"* | *source name* |
| **Counter-case, already works:** *…or "none found"* | *source name* |
| **Counter-case, could be hurt:** *…or "none found"* | *source name* |

## Prior learnings

| Past test | Result | How this design responds |
|---|---|---|
| *name* | *result with numbers, or status if not run yet* | *what changes because of it* |

*Or: "No related tests found."*

## Hypothesis

> If we *[change]*, *[metric]* will *[move]*, because *[evidence]*.

## What

- **In scope:** *the one change*
- **Out of scope:** *nearby ideas deliberately left out, each with a reason*

## Metrics

| Metric | Type | Goal |
|---|---|---|
| | Primary | |
| | Secondary | |
| | Guardrail | No drop |

## Experiment design

| Field | Value |
|---|---|
| **Type** | *A/B · A/B/n · ship and measure before/after* |
| **Control** | |
| **Treatment** | |
| **Audience** | *segment, markets, platforms* |
| **Exposure event** | *the event that puts a user in the test* |
| **MDE** | *minimum detectable effect: the smallest lift worth detecting* |
| **Estimated runtime** | |
| **Fallback** | *what we ship or test if the full change isn't feasible* |

## Risks & open questions

| Question | Owner | Needed for |
|---|---|---|
| | | |

## Sources

- *source name*

---

## Sizing formula

Two-arm test on a conversion metric, 80% power, α = 0.05:

- `n per arm ≈ 16 · p(1 − p) / δ²`, where `p` is the baseline rate and `δ` is the absolute lift to detect
- `runtime (weeks) = 2n / weekly eligible users`

Example: baseline 20%, detect +2 percentage points → `16 · 0.2 · 0.8 / 0.02² = 6,400` per arm.

Runtime over 4 weeks means the MDE is too small for the traffic. Say so, and propose a larger MDE or a wider audience.

## Pipeline row

The pipeline file holds one row per brief:

`| <order or —> | [<name>](<path to brief>) | <primary metric> | <status> | <blocker or None> |`

Status values:
- `Draft`: content not reviewed yet
- `Approved`: owner approved content and order
- `Running`: live. Needs MDE and runtime filled
- `Readout`: test ended, analysis in progress
- `Shipped` · `Killed`: final. Save the result where past experiments live, so the next brief finds it.
