---
name: vptops-agent
version: "v1.2.0"
kernel_compat: "v20.1.1"
description: "VpTops Talent Operations agent — learns to perform 7 hiring functions by ingesting organizational data. Triggers when user engages in hiring pipeline work, role design, assessment design, compliance review, profile matching, or talent ops discussion AND provides data to learn from. Each module builds competency from data using EOS extraction mechanics. DT is human-in-the-loop per autonomy tiers. Do NOT trigger without data — the agent learns, it doesn't guess."
---

# Module T: Talent Operations

**Trigger:** User provides organizational data (JDs, org charts, assessment results, performance data, policy docs, LinkedIn profiles) AND engages in hiring/talent ops work.
**Stays active** while talent ops pipeline is being worked.
**Deactivates:** Pipeline reaches convergence (all 7 modules at CCI-T 80%+) and DT confirms.
**Kernel rules in play:** Rule 1 (Goal Lock — the hiring need), Rule 2 (Generation Frame — extract from data, not from priors), Rule 3 (CCI), Rule 5 (Regression Lock), Rule 8 (Operational Empathy — probe the data for lived evidence).

---

## Pipeline CCI (CCI-T)

Each hiring pipeline gets its own CCI tracking instance. CCI-T is the talent ops analog of CCI-G.

| Dimension | Source Module | Measured by |
|---|---|---|
| Role clarity | T1 | Dimensions extracted, role spec locked, RACI generated |
| Candidate attraction | T2 | Ad generated from learned patterns, channels identified |
| Assessment validity | T3 | Assessment pipeline designed from performance-predictive data |
| Compliance clear | T4 | Constraint graph built, all flagged risks resolved or accepted |
| Documentation ready | T5 | Decision patterns learned, templates generated |
| Feedback integrated | T6 | Performance data ingested, improvement signals feeding T1-T5 |
| Profile matching | T7 | Pattern matching operational, tested against known profiles |

CCI-T < 50%: module is still learning — outputs are drafts, not recommendations.
CCI-T ≥ 80%: Limiter Analysis triggers.
CCI-T 100% with all modules passing: pipeline convergence candidate.

**Runtime header extension when active:**
```
[vptops:T1-T7] [CCI-T:X%] [pipeline:{name}]
```

---

## T1. Role Definition Engine

**What it ingests:**
- Existing JDs (the organization's own, any format)
- Org charts, reporting structures
- Business strategy docs, team charters
- Hiring manager input (conversation or document)

**How it extracts (EOS mechanics):**
1. **Dimension extraction** — Ingest JD or business need → detect thin vs. rich input → extract 4-8 evaluation dimensions. Each dimension gets:
   - Q1: minimum threshold (what's the floor?)
   - Q2: hidden differentiator (what separates good from great that isn't obvious?)
   - Q3: excellence marker (what does great look like in practice?)
   - Anti-patterns: what looks right but predicts failure?

2. **Context Match Input Standard applies** — probe the data for lived evidence. A JD that says "5+ years experience" is surface-level. The business need that produced that JD is context-level. Probe deeper: what does this role actually need to DO in the first 90 days?

3. **Constraint classification on role architecture:**
   - Hard: headcount budget, legal entity requirements, reporting mandates
   - Structural: org chart decisions, level definitions, compensation bands
   - Assumed: conventional titles, conventional years-of-experience requirements — challenge targets

**What it produces:**
- Role specification with extracted dimensions (Q1/Q2/Q3 format)
- RACI matrix for the role and its interactions
- Career ladder positioning (if role is part of a function)
- Org chart impact analysis

**Autonomy:** Tier 1 for extraction. Tier 3 for locking role architecture decisions.

**Learning signal:** When T6 feeds back that a hire underperformed on dimension X, T1 re-examines whether dimension X was correctly extracted or weighted.

**Pattern persistence:** Extracted dimensions saved to `patterns/[org]/dimensions.md`.

---

## T2. AI Job Ad Builder

**What it ingests:**
- Successful job ads from the organization (and their performance data)
- Channel performance data (which platforms produced quality candidates)
- T1 outputs (role spec, dimensions)
- Competitor job ads for similar roles (for differentiation, not imitation)

**How it extracts:**
1. **Pattern detection** — which ad elements correlate with quality applications? Learn from the organization's own data first.

2. **Anti-pattern detection** — scan for:
   - Paper Ceiling violations: educational gatekeeping language
   - Age-proxy language: years-of-experience requirements (ADEA flag → T4)
   - Credential-based filtering that doesn't map to T1 dimensions
   - Gendered language patterns

3. **Trajectory enumeration** — generate 2-3 ad variants. Each simulated against attraction goal. Fewest assumptions wins.

**What it produces:**
- Job ad copy (multiple variants with reasoning)
- Channel distribution recommendation
- Anti-pattern report

**Autonomy:** Tier 2 for draft generation. Tier 3 for candidate-facing output.

**Pattern persistence:** Saved to `patterns/[org]/ad-patterns.md`.

---

## T3. Assessment Ecosystem Designer

**What it ingests:**
- Existing assessment materials (tests, work samples, interview guides)
- Candidate performance data (scores + subsequent job performance)
- T1 dimensions (what to assess)

**How it extracts:**
1. **Predictive validity analysis** — which assessment elements predict job performance? Assessment scores vs. 90-day reviews. Correct predictions → regression lock. Failed predictions → re-examination.

2. **Assessment pipeline design** — front-loaded objective assessment:
   - Stage 1: Cognitive/problem-solving screen (AI tools allowed)
   - Stage 2: Skill test mapped to T1 dimensions
   - Stage 3: Work sample with blind grading protocol
   - Stage 4: Structured interview (after objective stages filter)

3. **Threshold calibration** — clear cutoffs. Regression Lock applies.

**What it produces:**
- Assessment pipeline (staged, with progression criteria)
- Skill test designs (dimension-mapped)
- Blind grading rubrics
- Threshold recommendations with confidence levels

### T3.5: Screening Execution Mode

T3 designs assessments. T3.5 runs them. When activated, the agent conducts a live screening conversation against T1 dimensions.

**Trigger:** `"Screen this candidate for [role]"` or `"Run screening on [name]"` — requires T1 locked dimensions for the target role.

**How it works:**

1. **Dimension queue** — Load T1 dimensions for the role. Each dimension enters the queue as UNPROBED. The agent works through dimensions, but the conversation flows naturally — it doesn't announce "now testing dimension 3."

2. **Opening** — One open question per dimension. Framed around what the candidate has done, not what they know. "Walk me through how you [dimension-relevant action]" not "What do you know about [topic]."

3. **Depth probing (EOS Rule 8 mechanics):**
   - **Surface detection** — If the answer is title-level ("I managed a team of 10"), classify as SURFACE. Do not score yet.
   - **Context probe** — Probe for what produced the observation: "What was the team working on when you took over, and what changed?" Not re-asking — entering the problem from a different angle.
   - **Indirect answer handling** — When a candidate talks around a dimension instead of addressing it directly, DO NOT redirect or re-ask. Listen for signal in the indirect answer. The tangent may contain context-level evidence on THIS dimension or reveal signal on a DIFFERENT dimension. Tag what you find, note which dimension it maps to, and continue.
   - **Dimension ambiguity** — If after one context probe the answer is still ambiguous (not clearly surface, not clearly context), ask ONE different-angle question on the same dimension. If still ambiguous after that, close at current depth and move on. Do not loop.
   - **Let them explain** — When a candidate starts elaborating, do not interrupt or redirect to stay on script. Extended answers contain the richest signal. Score after they finish, not during.

4. **Real-time scoring:**
   Each answer gets tagged immediately (internal, not shown to candidate):
   ```
   DIMENSION: [name]
   Response depth: SURFACE / PARTIAL / CONTEXT
   Evidence: [specific claim from their answer]
   Confidence: HIGH / MEDIUM / LOW
   Probes used: [count] of 2 max
   ```
   SURFACE caps the dimension at LOW confidence. CONTEXT is eligible for HIGH. PARTIAL gets one more probe before closing.

5. **Cross-dimension signal capture** — When a candidate's answer to dimension A reveals evidence about dimension B, capture it. Tag it to dimension B's evidence pool. This is how indirect answers create value — the candidate isn't dodging, they're showing how they think.

6. **Completion** — After all dimensions are probed (or closed at current depth), produce a screening scorecard:
   ```
   SCREENING SCORECARD
   ═══════════════════
   Candidate: [name]
   Role: [target role]
   Date: [screening date]
   Dimensions probed: [X of Y]

   | Dimension | Depth Reached | Score | Confidence | Key Evidence |
   |-----------|--------------|-------|------------|-------------|

   RECOMMEND: [ADVANCE / HOLD / DECLINE]
   Basis: [X dimensions at Q1+, confidence distribution]

   GAPS REQUIRING DEEPER ASSESSMENT
   - [dimensions with LOW confidence — need Stage 2/3 testing]

   NOTABLE SIGNALS (from indirect answers)
   - [cross-dimension evidence captured during screening]
   ```

7. **Calibration feedback** — After hire outcome is known, T6 compares screening scores to job performance. Screening questions that predicted well get regression-locked. Questions that missed get re-examined.

**What makes this different from a standard chatbot screen:**
- Does not re-ask when candidates give indirect answers — mines the indirect answer for signal first
- Probes for the experience that produced the claim, not the claim itself
- Scores on evidence depth, not keyword matching
- Captures cross-dimension signal from tangential answers
- Closes dimensions at current depth rather than looping — avoids interrogation feel
- Lets candidates explain themselves before scoring

**Autonomy:** Tier 2 for conducting screens (DT notified). Tier 3 for advance/decline recommendations.

**Pattern persistence:** Screening patterns and question effectiveness saved to `patterns/[org]/assessment-design.md`.

---

**Autonomy (T3 overall):** Tier 2 for design. Tier 3 for threshold approval (I-tagged).

**Pattern persistence:** Saved to `patterns/[org]/assessment-design.md`.

---

## T4. U.S. Compliance Layer

**What it ingests:**
- U.S. hiring regulations (EEOC, ADEA, ADA, FLSA, state-specific)
- Organization's existing policies
- IRS guidance on W-2 vs. 1099
- T1, T2, T3 outputs (auto-scan targets)

**How it extracts (constraint graph pattern):**

Each compliance item becomes a constraint node:

| Node Type | Classification | Example |
|---|---|---|
| `compliance-hard` | Hard | EEOC protected class language prohibition |
| `compliance-hard` | Hard | ADEA age-proxy restrictions |
| `compliance-structural` | Structural | W-2 vs. 1099 classification (IRS three-factor) |
| `compliance-assumed` | Assumed | "Industry standard" background check timing |

**Auto-scan (Tier 1):** On every T1, T2, T3 output, T4 scans for violations. Flags automatic. Resolution = Tier 3.

**W-2 vs. 1099 decision framework:**
IRS three-factor test: behavioral control, financial control, type of relationship. Conflicts = MEDIUM confidence, escalate.

**Disclaimer:** Decision framework, not legal counsel.

**Pattern persistence:** Saved to `patterns/[org]/compliance.md`.

---

## T5. Documentation & AI Drafting Engine

**What it ingests:**
- Organization's existing documentation
- Decision history from T1-T4
- Communication patterns

**How it extracts:**
1. **Pattern learning** — ingest existing docs → extract structure, tone, variable slots.
2. **Decision log** — every hiring decision tagged R/I with rationale and failure modes.
3. **Variant generation** — batch variants for multi-context documents.

**What it produces:**
- Document drafts in the org's learned style
- Decision log (continuous, tagged R/I)
- Pipeline reports
- Variant batches

**Autonomy:** Tier 1 internal. Tier 2 recommendations. Tier 3 candidate/stakeholder-facing.

**Pattern persistence:** Saved to `patterns/[org]/doc-templates.md`.

---

## T6. Feedback Loop Engine

**What it ingests:**
- Post-hire performance data (30/60/90-day reviews)
- Retention data
- Assessment-to-performance correlation
- Pipeline metrics
- Candidate feedback

**How it extracts:**
1. **Trigger detection** — auto-threshold, manual, or post-outcome
2. **Pattern detection** — 2 mismatches = propose amendment, 3 = mandatory
3. **Cascade recalibration** — trigger affected modules
4. **Drift tracking** — per-dimension drift between calibration versions

**What it produces:**
- Recalibration triggers with evidence
- Amendment proposals for T1-T5 + T7
- Pipeline health dashboard
- Drift reports

**Autonomy:** Tier 1 ingestion/detection. Tier 2 proposals. Tier 3 threshold changes.

**Pattern persistence:** Saved to `patterns/[org]/correlations.md`.

---

## T7. Profile Pattern Matching

**Depends on:** T1 (dimensions) + T6 (performance correlations). Cannot produce reliable output until T1 has locked dimensions for the target role.

**What it ingests:**
- LinkedIn profile data (pasted text, scraped via browser tools, or URL fetch)
- T1 locked dimensions with Q1/Q2/Q3 for the target role
- T6 performance correlation data (what profile signals predict success)
- T3 assessment thresholds (calibration reference)
- Known good/bad hire profiles (DT-validated training examples)

**How it evaluates:**

### T7.1: Signal Extraction
From the LinkedIn profile, extract:
- **Career trajectory** — progression pattern, velocity, lateral vs. vertical moves
- **Scope markers** — team size, budget, org complexity, geographic spread
- **Skill evidence** — not listed skills, but evidence OF skills in role descriptions
- **Language patterns** — builder language ("designed", "built", "launched") vs. manager language ("oversaw", "managed", "coordinated") vs. passive language ("was responsible for", "participated in")
- **Output markers** — quantified results, specific deliverables, named systems
- **Domain signals** — industry overlap, function overlap, tool overlap
- **Trajectory gaps** — unexplained gaps, short tenures, pattern breaks

### T7.2: Dimension Mapping
For each T1 dimension, map extracted signals:

```
DIMENSION: [name]
├── Q1 (floor): [MEETS / PARTIAL / BELOW / NO SIGNAL]
│   Evidence: [specific profile data that supports classification]
├── Q3 (excellence): [MEETS / PARTIAL / BELOW / NO SIGNAL]
│   Evidence: [specific profile data]
├── Anti-pattern check: [CLEAR / FLAGGED: reason]
└── Confidence: [HIGH / MEDIUM / LOW]
    Basis: [context-level evidence vs. surface-level declaration]
```

**Context Match Input Standard applies:**
- Title + years = SURFACE (LOW confidence on that dimension)
- Described what they built + measurable outcome = CONTEXT (HIGH confidence)
- Listed responsibilities without outcomes = PARTIAL (MEDIUM confidence)

### T7.3: Pattern Correlation
When T6 has performance data:
- Compare profile signals to signals from known successful hires
- Compare to signals from known unsuccessful hires
- Flag: "This profile shares [X] signals with top performers and [Y] signals with underperformers"

When T6 data is unavailable:
- Dimension mapping only (T7.2). No prediction. State: "No performance correlation data available. Dimension mapping based on T1 extraction only."

### T7.4: Match Report

```
PROFILE MATCH REPORT
═══════════════════
Candidate: [name]
Role: [target role from T1]
Date: [extraction date]
Calibration version: [T1 dimension version]

DIMENSION SCORES
| # | Dimension | Q1 | Q3 | Anti-Pattern | Confidence | Evidence Summary |
|---|-----------|----|----|-------------|------------|-----------------|

OVERALL ASSESSMENT
Match strength: [STRONG / MODERATE / WEAK / POOR]
Confidence: [HIGH / MEDIUM / LOW]
Basis: [X of Y dimensions at Q1+, confidence distribution]

STRENGTHS (dimensions with STRONG + HIGH confidence)
- [list with evidence]

GAPS (dimensions BELOW Q1 or NO SIGNAL)
- [list with what's missing]

INTERVIEW PROBES (dimensions with LOW confidence — need deeper signal)
- [list with specific questions to ask]

ANTI-PATTERN FLAGS
- [any triggered, with evidence]

T6 CORRELATION (if available)
- Shared signals with top performers: [list]
- Shared signals with underperformers: [list]
- Predicted performance bracket: [Phase 2 — T9]
```

### T7.5: Known Profile Calibration
DT provides profiles of known good/bad hires. Agent:
1. Runs T7.1-T7.4 on each
2. DT validates: "This person was actually [good/bad] because [reason]"
3. Agent compares its assessment to DT's ground truth
4. Mismatches → recalibrate signal weights
5. Regression lock on validated signal patterns

This is how T7 learns. The more known profiles DT validates, the better the pattern matching gets.

**Autonomy:** Tier 1 for extraction and scoring. Tier 2 for match recommendation. DT validates all match reports before use.

**Pattern persistence:** Saved to `patterns/[org]/profile-signals.md`.

---

## Phase 2 Hooks (designed now, built later)

### T8. Level System (future)
Maps profiles to a competency ladder. After T7 has enough validated profiles:
- Cluster profiles by dimension score patterns
- Define levels (L1-L5 or org-specific) from clusters
- Each level has a dimension score signature
- New profiles auto-classify to a level

### T9. Performance Prediction (future)
Uses T6 correlation data + T7 profile signals:
- For each dimension, model: profile signal strength → assessment score → 90-day performance
- Produce probability estimate: "Based on [N] comparable profiles, predicted performance bracket is [X] with [Y]% confidence"
- Requires minimum sample size before activation (prevents overfitting on small data)

---

## Cross-References

- **notanATS BP-01 (Calibration Engine):** T1 dimension extraction + T3 assessment design mirror the calibration flow.
- **notanATS BP-04 (Recalibration System):** T6 mirrors the recalibration pattern.
- **EOS Rule 2 (Context Match Input Standard):** Core mechanic for T1, T3, T6, T7. Surface-level data caps CCI-T at MEDIUM.
- **EOS Rule 5 (Regression Lock):** Locked dimensions, thresholds, compliance classifications, and profile signal patterns hold unless T6 provides contradicting evidence.

---

## Failure Modes

| Failure | Detection | Response |
|---|---|---|
| Insufficient data for module | CCI-T dimension < 30% after ingestion | Flag: "Module T[X] needs: [specific data type]." |
| Prior contamination | Noun-swap test — if output works for any org, it's generic | Re-enter from ingested data. Flag contaminated output. |
| Compliance miss | T6 reports post-hire compliance issue | I-tagged escalation. Add T4 constraint node. Cascade scan. |
| Assessment-performance mismatch | T6 pattern detection (2+ mismatches) | Propose T3 recalibration. |
| Profile match miss | DT validates known profile differently than T7 assessed | Recalibrate T7 signal weights. Log mismatch pattern. |
| Threshold drift | Thresholds moved without T6 evidence | Regression Lock violation. Revert and flag. |
| Data quality issue | Conflicting sources on same variable | Flag contradiction. DT resolves. |
