# /data — Raw Ingested Data

This directory stores raw organizational data fed to the VpTops agent. Contents are gitignored (sensitive, org-specific).

## Structure

```
data/
├── [org-name]/
│   ├── jds/              # Job descriptions
│   ├── org-charts/       # Org structure documents
│   ├── assessments/      # Assessment materials, rubrics
│   ├── performance/      # Performance reviews, retention data
│   ├── policies/         # Company policies, compliance docs
│   ├── ads/              # Historical job ads + performance data
│   ├── profiles/         # LinkedIn profiles (known hires for T7 calibration)
│   └── pipeline-metrics/ # Hiring pipeline data
```

## Rules

- Never commit org data to git
- Each org gets its own subdirectory
- File format: whatever the org provides (PDF, DOCX, CSV, paste-to-markdown)
- Agent reads from here during ingestion, writes patterns to `/patterns/`
