# VpTops — AI-First Hiring System

An AI agent that designs and operates a complete hiring pipeline. It ingests organizational data — JDs, org charts, assessments, performance reviews, compliance policies, candidate profiles — and builds the competency to run each hiring function from that data. Nothing is pre-loaded. Every output traces back to ingested evidence.

The system uses post-hire performance and retention data as signals to continuously improve role design, assessments, and pipelines. Each hiring cycle produces better signal than the last.

## System Architecture

VpTops runs on two layers:

1. **EOS kernel** (`eos/kernel.md`) — the extraction engine. Controls how the agent reasons: context displacement over training priors, mandatory trajectory simulation, regression-locked decisions, tiered human oversight. The kernel ensures the agent generates from your data, not from generic hiring conventions.

2. **Talent Operations skill** (`.claude/skills/vptops-agent/SKILL.md`) — seven learning modules that build competency from ingested data. Each module starts empty and earns its outputs through data coverage (CCI-T tracking).

## What the System Operates

| Module | What It Does | How It Works |
|--------|-------------|-------------|
| **T1** | Design role specifications, org charts, ladders, and RACIs that translate real business needs into clear hiring targets | Ingests JDs and business needs → extracts 4-8 evaluation dimensions with floor/differentiator/excellence markers per dimension. Produces RACI matrices and org chart impact analysis. |
| **T2** | Build AI-assisted job ads and distribution systems that attract the right candidates without manual sourcing | Learns which ad elements correlate with quality applications from org data. Scans for Paper Ceiling, age-proxy, and credential-gatekeeping anti-patterns. Generates variants with channel recommendations. |
| **T3** | Design and run screening and assessment ecosystems that evaluate skills, judgment, and transferable experience beyond resumes | Builds staged assessment pipelines mapped to T1 dimensions. Front-loads objective evaluation before structured interviews. T3.5 screening mode conducts live candidate conversations — probes for context-level evidence, mines indirect answers for cross-dimension signal, and scores on evidence depth rather than keyword matching. Calibrates thresholds against post-hire performance. |
| **T4** | Apply working knowledge of U.S. hiring practices to guide stakeholders and flag risk (W-2 vs. 1099, EEOC, ADEA) | Maintains a constraint graph of EEOC, ADEA, ADA, FLSA, and W-2/1099 nodes. Auto-scans every T1-T3 output for violations at Tier 1 autonomy. Decision frameworks, not legal advice. |
| **T5** | Document thinking, research, and decisions, then leverage AI to generate drafts, variants, and analyses at scale | Learns org documentation patterns, generates drafts in that style. Maintains tagged decision logs (routine vs. irreversible). Produces variant batches for multi-context documents. |
| **T6** | Use post-hire performance and retention data as signals to continuously improve role design, assessments, and pipelines | Ingests 30/60/90-day reviews and retention data. Detects assessment-to-performance mismatches (2 = propose amendment, 3 = mandatory). Triggers cascade recalibration across T1-T5. |
| **T7** | Match candidate profiles against learned success patterns extracted from T1 dimensions and T6 performance correlations | Extracts career trajectory, scope markers, skill evidence, and language patterns from profiles. Maps signals to T1 dimensions. Calibrates against known good/bad hires. |

T6 is the feedback mechanism that makes this a closed loop — hire outcomes recalibrate T1 through T5 and sharpen T7 signal extraction with every validated profile.

**Phase 2:** Level System (T8) clusters validated profiles into org-specific competency levels. Performance Prediction (T9) models profile signal to job performance probability. Both require minimum sample sizes before activation.

## How It Works

```
Org data in  →  Agent extracts patterns  →  Human validates  →  Patterns lock
                                                                      ↓
Hire outcomes  →  T6 detects signal  →  Cascade recalibration  →  Tighter patterns
```

The agent produces nothing from modules that haven't ingested data. The noun-swap test enforces specificity: if an output works for any organization with different names substituted, it's rejected and re-entered from data.

Human oversight is tiered:
- **Tier 1** — Extraction and pattern detection run autonomously
- **Tier 2** — Recommendations and match reports require notification
- **Tier 3** — Strategic decisions, threshold changes, and candidate-facing output require confirmation

## Running It

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and authenticated
- EOS kernel installed in `~/.claude/CLAUDE.md` (reference copy included at `eos/kernel.md`)

Optional integrations for full capability:
- Notion (MCP server) — persistence layer for learned patterns and state
- Google Drive (MCP server) — direct doc ingestion
- Chrome extension (Claude in Chrome) — LinkedIn profile scraping
- Pieces (MCP server) — supplementary long-term memory

### Bootstrap

```bash
cd VpTops
claude
```

```
Bootstrap VpTops on [org name]
```

The agent inventories available data, classifies each source by target module, and produces a Data Coverage Report showing what it can learn now and what's missing.

### Ingestion

Feed data in any format — paste, Google Drive link, Notion URL, spreadsheet upload, LinkedIn URL. The agent classifies and routes to the correct module(s) automatically.

```
"Here's our data: [source]"                   Ingest and route
"Run T1 on this JD: [paste/link]"             Extract role dimensions
"Match this profile: [LinkedIn URL]"          T7 profile match report
"This person was [good/bad] because [X]"      T7 calibration with known hire
"Feed performance data: [source]"             T6 feedback loop
"Show coverage report"                        Module-by-module data coverage
"Show pipeline status"                        CCI-T across all modules
```

## Project Structure

```
VpTops/
  eos/
    kernel.md                                 EOS extraction engine (reference copy)
  .claude/skills/vptops-agent/SKILL.md        The agent — all 7 modules
  docs/
    bootstrap.md                              First-run workflow (detailed)
    ingestion.md                              Data classification and routing spec
    profile-matching.md                       T7 signal extraction and calibration spec
  patterns/[org]/                             Learned knowledge per organization
    dimensions.md                             T1 — role dimensions, Q1/Q2/Q3, anti-patterns
    ad-patterns.md                            T2 — ad elements correlated with quality applications
    assessment-design.md                      T3 — assessment pipeline, thresholds, blind grading
    compliance.md                             T4 — constraint graph, W-2/1099, EEOC/ADEA nodes
    doc-templates.md                          T5 — organizational documentation patterns
    correlations.md                           T6 — assessment-to-performance correlations, drift
    profile-signals.md                        T7 — signal weights calibrated against known hires
  data/[org]/                                 Raw ingested data (gitignored)
  CLAUDE.md                                   Project-level agent instructions
```

## Connection to notanATS

VpTops is the strategic layer — it designs role architecture, assessment pipelines, compliance constraints, and profile matching models. notanATS is the product layer — it executes what VpTops designs. T1 dimensions feed notanATS calibration. T6 feedback triggers notanATS recalibration.
