# VpTops — Talent Operations Agent

## What This Is

An AI agent that learns to perform talent operations by ingesting organizational data. It doesn't come pre-loaded with hiring expertise — it builds competency from the data you feed it. EOS methodology is the extraction engine.

7 learning modules: Role Definition (T1), Job Ads (T2), Assessment Design (T3), U.S. Compliance (T4), Documentation (T5), Feedback Loop (T6), Profile Matching (T7).

Phase 2 (future): Level System (T8), Performance Prediction (T9).

## How To Use

1. Open a session in this project directory
2. Say: `Bootstrap VpTops on [org name]`
3. Feed it data (JDs, org charts, assessments, performance data, policies)
4. Agent extracts patterns, you validate and lock
5. Feed it LinkedIn profiles — it matches against learned patterns
6. Feed it known good/bad hires — it calibrates its pattern matching

## Key Files

| File | Purpose |
|---|---|
| `.claude/skills/vptops-agent/SKILL.md` | The agent — all 7 modules |
| `docs/ingestion.md` | How data gets consumed and routed |
| `docs/bootstrap.md` | First-run guide |
| `docs/profile-matching.md` | T7 profile matching spec |
| `patterns/[org]/` | Learned knowledge per organization |
| `data/[org]/` | Raw ingested data (gitignored) |

## Commands

```
"Bootstrap VpTops on [org name]"              → Start learning from org data
"Here's our data: [source]"                   → Ingest data
"Show coverage report"                        → What modules have learned
"Run T1 on this JD: [paste/link]"             → Extract role dimensions
"Match this profile: [paste/URL]"             → T7 profile match report
"This person was [good/bad] because [X]"      → T7 calibration with known profile
"Show pipeline status"                        → CCI-T all modules
"Feed performance data: [source]"             → T6 feedback loop
```

## Rules

- Agent produces NOTHING from modules that haven't ingested data. No guessing.
- All outputs run the noun-swap test: if the output works for any org, it's generic — re-enter from data.
- Pattern persistence: extracted patterns saved to `patterns/[org]/` and survive sessions.
- DT is human-in-the-loop: Tier 1 (extraction), Tier 2 (recommendations), Tier 3 (strategic decisions, candidate-facing output).
- Compliance outputs are decision frameworks, not legal advice.

## Connection to notanATS

VpTops is the strategic layer. notanATS is the product layer. T1 dimensions → notanATS calibration dimensions. T6 feedback → notanATS recalibration. Agent designs what the product executes.
