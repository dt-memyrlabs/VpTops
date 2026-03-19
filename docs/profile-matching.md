# T7: Profile Pattern Matching — Specification

## Overview

After T1-T6 have ingested org data and built patterns, T7 evaluates candidate profiles (primarily LinkedIn) against learned dimensions. It answers: does this person's observable profile signal the competencies this role requires?

## Dependencies

- **T1 locked dimensions** — REQUIRED. T7 cannot match without knowing what to match against.
- **T6 performance correlations** — OPTIONAL but improves accuracy. Without T6, matching is dimension-only. With T6, matching includes predictive signals from known hire outcomes.
- **Known profile calibration** — OPTIONAL but critical for accuracy. DT provides profiles of known good/bad hires, validates T7's assessment, agent recalibrates.

## Signal Extraction (T7.1)

From any LinkedIn profile, extract these signal categories:

### Career Trajectory
- Progression velocity (how fast did they move up?)
- Lateral vs. vertical moves (breadth vs. depth)
- Company tier progression (startup → mid → enterprise, or reverse)
- Function consistency vs. pivots
- Tenure patterns (average stay, trend direction)

### Scope Markers
- Team/org size managed
- Budget responsibility
- Geographic spread
- Cross-functional vs. single-function
- Revenue/P&L impact

### Skill Evidence
Not "listed skills" — evidence of skills demonstrated in role descriptions:
- Builder language: "designed", "built", "launched", "created", "architected"
- Manager language: "oversaw", "managed", "coordinated", "led"
- Passive language: "was responsible for", "participated in", "assisted with"
- Output language: specific deliverables named, systems built, results quantified

### Output Markers
- Quantified results (revenue, efficiency, growth metrics)
- Named systems/products/initiatives
- Before/after comparisons
- Scale indicators (users, transactions, headcount)

### Domain Signals
- Industry overlap with target role
- Function overlap
- Tool/technology overlap
- Regulatory environment overlap

### Anti-Signals
- Unexplained gaps > 6 months
- Pattern of short tenures (< 1 year repeated)
- Title inflation without scope evidence
- All passive language, no output markers
- Skills listed but never demonstrated in role descriptions

## Dimension Mapping (T7.2)

For each T1 dimension, map extracted signals to Q1/Q3:

| Score | Meaning | Evidence Required |
|---|---|---|
| **STRONG** | Profile demonstrates this dimension clearly | Context-level: specific outcomes, built systems, quantified results |
| **PARTIAL** | Some signal present but incomplete | Mix of context and surface: title implies it, some evidence supports |
| **WEAK** | Minimal or indirect signal | Surface-level only: title/years suggest it, no demonstration |
| **NO SIGNAL** | Profile shows nothing on this dimension | Absence — might exist but isn't visible |

Confidence per dimension:
- **HIGH** — context-level evidence (what they built + outcome)
- **MEDIUM** — surface-level evidence (title + responsibilities listed)
- **LOW** — inference only (industry overlap suggests possible exposure)

## Known Profile Calibration (T7.5)

The most important learning mechanism. DT provides:
1. A LinkedIn profile of a known hire
2. Their actual performance outcome: good/bad/mixed
3. The specific reason: "They excelled at X but failed at Y because Z"

Agent:
1. Runs full T7.1-T7.4 analysis
2. Compares its assessment to DT's ground truth
3. Identifies where its signal extraction was right vs. wrong
4. Adjusts signal weights: signals that predicted correctly get reinforced, signals that missed get flagged
5. After 5+ calibration profiles: pattern model is production-grade

This is how VpTops learns YOUR hiring patterns, not generic ones.

## Phase 2: Level System (T8) — Design Notes

After T7 has 10+ validated profiles:
- Cluster profiles by dimension score patterns
- Define levels from clusters (the data defines the levels, not arbitrary ladders)
- Each level has a dimension score signature
- New profiles auto-classify to nearest level
- Levels are org-specific — they emerge from that org's data

## Phase 2: Performance Prediction (T9) — Design Notes

After T6 has 10+ hire outcomes AND T7 has 10+ validated profiles:
- For each dimension: model profile signal → assessment score → job performance
- Produce probability: "Based on [N] comparable profiles, predicted performance: [bracket] at [confidence]%"
- Minimum sample size enforced — no predictions on small data
- Prediction accuracy tracked and recalibrated via T6
