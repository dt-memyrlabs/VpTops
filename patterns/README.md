# /patterns — Learned Knowledge

This directory stores the agent's extracted patterns — the knowledge it builds from ingested data. This IS the agent's competency, persisted as markdown.

## Structure

```
patterns/
├── [org-name]/
│   ├── dimensions.md         # T1: Extracted role dimensions (Q1/Q2/Q3)
│   ├── ad-patterns.md        # T2: What works in job ads
│   ├── assessment-design.md  # T3: Assessment pipelines and thresholds
│   ├── compliance.md         # T4: Compliance constraint graph
│   ├── doc-templates.md      # T5: Documentation patterns and templates
│   ├── correlations.md       # T6: Performance correlation data
│   └── profile-signals.md    # T7: Profile signal patterns (what predicts success)
```

## Rules

- Patterns ARE committed to git (they're the reusable knowledge)
- Each file is versioned: calibration version N
- Regression-locked per EOS Rule 5: patterns hold unless T6 provides contradicting evidence
- When recalibration happens, previous version preserved (version N → N+1)
- The noun-swap test applies: if a pattern file works for any org, it's generic and invalid
