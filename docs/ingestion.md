# VpTops Data Ingestion Protocol

**Purpose:** Defines how the VpTops agent consumes data sources, classifies them, and routes them to the correct learning module (T1-T7).

**Principle:** The agent learns from data, not from priors. Every module's CCI-T dimension starts at 0% and rises as data is ingested and patterns are extracted. No output is generated from a module that hasn't ingested relevant data.

---

## Source Types

| Source | Access Method | Routes To |
|---|---|---|
| Job descriptions (existing) | Paste, Google Drive, Notion | T1, T2 |
| Org charts / reporting structures | Paste, Google Drive, image | T1 |
| Business strategy / team charters | Google Drive, Notion, paste | T1 |
| Job ads (historical + performance) | Paste, Google Drive | T2 |
| Channel performance data | Spreadsheet, paste | T2 |
| Assessment materials (tests, rubrics) | Google Drive, paste | T3 |
| Candidate scores + outcomes | Spreadsheet, database, paste | T3, T6 |
| U.S. hiring regulations / policies | Web fetch, paste, Google Drive | T4 |
| Company policies / employee handbook | Google Drive, paste | T4 |
| Document templates (offers, rejections) | Google Drive, Notion | T5 |
| Decision logs / hiring notes | Notion, paste | T5 |
| Performance reviews (30/60/90) | Spreadsheet, Google Drive, paste | T6 |
| Retention data | Spreadsheet, paste | T6 |
| Exit interview data | Paste, Google Drive | T6 |
| Pipeline metrics | Spreadsheet, paste | T6, T2, T3 |
| LinkedIn profiles | Paste, browser scrape, URL fetch | T7 |
| Known hire profiles (validated) | Paste with DT assessment | T7 (calibration) |

---

## Ingestion Flow

```
Data arrives (any source)
        │
        ▼
   CLASSIFY
        ├── What type of data is this?
        ├── Which module(s) does it feed?
        ├── What's the data quality? (complete/partial/conflicting)
        └── Is this surface-level or context-level?
        │
        ▼
   ROUTE to target module(s)
        │
        ▼
   MODULE EXTRACTION
        ├── Module applies its extraction logic (per T1-T7 specs)
        ├── Extracted patterns tagged with source and confidence
        └── CCI-T dimension updated
        │
        ▼
   PATTERN PERSISTENCE
        ├── Save extracted patterns to patterns/[org]/[module].md
        └── Version tag: calibration version N
        │
        ▼
   CROSS-MODULE PROPAGATION
        ├── If data feeds multiple modules, each receives independently
        ├── If extraction reveals compliance implications → auto-route to T4
        ├── If extraction reveals feedback signal → auto-route to T6
        └── If T1 dimensions update → auto-notify T7 (profile matching recalibrates)
```

---

## Quality Classification

| Quality Level | Definition | CCI Impact |
|---|---|---|
| **Context-level** | Traceable to specific outcomes, lived experience, or measured results | Full CCI credit |
| **Surface-level** | Declarative without outcome data | Partial CCI credit, caps at MEDIUM |
| **Conflicting** | Multiple sources disagree on same variable | CCI frozen until DT resolves |
| **Insufficient** | Too sparse for pattern extraction | CCI stays below 30%, outputs flagged as drafts |

---

## Batch Ingestion

For initial bootstrap:
1. DT provides batch of data sources
2. Agent classifies and routes each item
3. Per-module CCI-T updates as each is processed
4. After batch: **Data Coverage Report** showing gaps and recommendations

---

## Pattern Persistence

All extracted patterns saved to `patterns/[org-name]/`:
- `dimensions.md` — T1
- `ad-patterns.md` — T2
- `assessment-design.md` — T3
- `compliance.md` — T4
- `doc-templates.md` — T5
- `correlations.md` — T6
- `profile-signals.md` — T7

Each file versioned with calibration number. Regression-locked per EOS Rule 5.

---

## Integration Points

| Tool | Usage |
|---|---|
| `google_drive_search` / `google_drive_fetch` | Ingest docs from Google Drive |
| `notion-search` / `notion-fetch` | Ingest from Notion databases and pages |
| `notion-create-pages` / `notion-update-page` | Write extraction results, decision logs |
| Spreadsheet tools (xlsx skill) | Ingest performance data, metrics, scores |
| `WebFetch` | Ingest public content, LinkedIn profiles, regulations |
| Browser tools (Chrome MCP) | Scrape LinkedIn profiles |
| `create_pieces_memory` | Persist critical extraction results |
