# VpTops Bootstrap Workflow

**Purpose:** First-run guide. Point the agent at an organization's data, build competency, validate what it learns.

---

## Prerequisites

- EOS kernel v20.0.0+ active
- VpTops project directory as working directory
- At least one data source available
- Goal locked: the specific hiring need

---

## Phase 1: Data Inventory (CCI-T: 0% → 15%)

**DT action:** Provide available data sources.

**Agent action:**
1. Classify each source (type, quality, target module)
2. Produce **Data Coverage Report** (T1-T7 per module)
3. Identify critical gaps
4. Recommend what to provide next for maximum CCI-T lift

---

## Phase 2: Module Bootstrap (CCI-T: 15% → 50%)

Run in dependency order:

### 2a: T1 — Role Definition (first, everything depends on it)
Input: JDs + business context → Extract dimensions, constraints, RACI

### 2b: T4 — Compliance (parallel with T1)
Input: Regulations + policies → Build constraint graph, W-2/1099 classification

### 2c: T2 — Job Ads (needs T1 + T4)
Input: T1 dimensions + historical ads → Extract ad patterns, anti-patterns

### 2d: T3 — Assessments (needs T1 + T4)
Input: T1 dimensions + assessment materials → Design pipeline, thresholds

### 2e: T5 — Documentation (needs T1-T4 decisions)
Input: Existing templates + decision log → Learn org's documentation style

### 2f: T7 — Profile Matching (needs T1 locked)
Input: T1 locked dimensions + known profiles → Calibrate signal extraction

DT validates each module's output before locking.

---

## Phase 3: Pipeline Activation (CCI-T: 50% → 80%)

All modules have baseline competency. Pipeline operational.

Agent produces: complete pipeline report, ready-to-use outputs.
DT validates: outputs usable, no unresolved flags.
T6 begins accepting performance data. T7 operational for profile screening.

---

## Phase 4: Feedback Integration (CCI-T: 80% → 100%)

Runs over time as hires are made:
- T6 ingests performance data → pattern detection → cascade recalibration
- T7 calibrates against DT's known-profile validations
- System self-improves

---

## Quick Reference

```
"Bootstrap VpTops on [org name]"              → Phase 1
"Here's our data: [source]"                   → Ingestion
"Show coverage report"                        → Data Coverage Report
"Run T1 on this JD: [paste/link]"             → T1 extraction
"Match this profile: [LinkedIn paste/URL]"    → T7 profile match
"This person was [good/bad] because [reason]" → T7 calibration
"Show pipeline status"                        → CCI-T all modules
"Feed performance data: [source]"             → T6 ingestion
"Show what changed since last calibration"    → Drift report
```
