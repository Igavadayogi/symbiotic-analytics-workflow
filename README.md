# Symbiotic Analytics Workflow
### A Claude Code-First Method for Data Analytics Projects

**Author:** Igusti Agung Vadayogi Raharja  
**GitHub:** [github.com/Igavadayogi](https://github.com/Igavadayogi)  
**LinkedIn:** [linkedin.com/in/vadayogi](https://linkedin.com/in/vadayogi)

---

## What This Is

This repo documents the methodology I use to deliver end-to-end analytics projects using Claude Code as the primary execution engine.

This is not a "use AI to write code faster" workflow. It is a structured system where:

- **Claude.ai** acts as strategic advisor and prompt architect
- **Claude Code** handles all mechanical execution (cleaning, EDA, aggregation, output)
- **The analyst** owns all business logic decisions, validation gates, and client-facing judgments

The method is built around one core principle: **the quality of the output is directly proportional to the quality of the prompt.** Invest in the prompt. Never fix bad output — fix the prompt that produced it.

---

## The Three-Lane System

Every session runs three lanes simultaneously. None of them block each other.

| Lane | Role | Handles |
|------|------|---------|
| Claude.ai | Strategic advisor + prompt architect | Problem framing, prompt design, findings validation, judgment calls |
| Claude Code | Execution engine | Data cleaning scripts, EDA aggregations, file outputs, dashboard code |
| Analyst (you) | Business logic owner + sign-off authority | Gate validation, domain judgment, client communication, findings interpretation |

The critical distinction: **Claude Code can execute anything. Knowing what to execute, in what order, and what the output means for the business — that is what the analyst brings.**

---

## The Six-Stage Workflow

Every project follows the same six stages. Claude Code is integrated at every execution stage.

| Stage | Name | What Happens | Claude Code Role |
|-------|------|--------------|-----------------|
| 1 | Data Contracting | Define schema, expected values, validation rules before touching data | Generate validation scripts from analyst-approved contracts |
| 2 | Data Cleaning | Standardize, deduplicate, correct, flag | Execute 6-step pipeline; produce cleaning log with row-level audit trail |
| 3 | EDA | Exploratory analysis; surface ranked findings for analyst review | Run full analysis; output structured CSVs for analyst sign-off |
| 4 | Predictive Modeling | Segmentation, churn prediction, statistical tests | Generate model code; output scored datasets and evaluation metrics |
| 5 | Semantic Layer + Dashboard | Aggregate to dashboard-ready format; build visualization | Build semantic layer CSVs; generate HTML dashboard or BI tool prep files |
| 6 | Synthesis | Structure narrative; draft client-facing copy | Format findings; draft executive summary in required language |

**The analyst signs off between every stage before the next one begins. Nothing advances without explicit approval.**

---

## The Five-Part Prompt Architecture

Every Claude Code prompt in this workflow follows the same structure. This is the most important part of the method — the structure is what eliminates ambiguity and prevents compounding errors.

```
CONTEXT
  What the data is, where it came from, what state it is in right now.
  Include: dataset name, row count, column names, known issues, prior transformations.

TASK
  Exactly what to compute, in what order.
  Use numbered steps. Each step produces a named output.

LOGIC
  Explicit rules for every derived field, threshold, or classification.
  Example: health_status = KRITIS if negative_pct >= 40%, PERHATIAN if >= 20%
  Never leave business logic implicit — Claude Code will guess, and the guess will look correct.

OUTPUT
  Exact filenames. Exact column names. Exact format (CSV, HTML, JSON).
  Include a sample row if the format is non-obvious.

CONSTRAINT
  What NOT to do.
  Example: do not remove rows based on near-duplicate flags — only flag them for review.
  Example: do not interpret findings — output numbers only, interpretation happens in the next stage.
```

**The constraint block is as important as the task block.** An unbounded prompt produces output that handles edge cases in ways you did not expect, did not review, and cannot audit.

---

## The Three Validation Gates

Nothing moves to the next stage without passing its gate. These are non-negotiable.

### Gate 1 — Data Quality Sign-Off
Triggered after Stage 2 (Data Cleaning). Before any analysis runs on the dataset.

The analyst must confirm:
- All expected entities are present with sufficient row counts
- Sentiment labels or categorical fields are internally consistent
- Date ranges are complete and plausible
- Duplicate and near-duplicate decisions have been made with manual inspection, not auto-removed
- Any anomalies are documented (not silently dropped)

**Gate 1 failure mode:** Running EDA on uncleaned data and not discovering the problem until the client asks a question the data cannot answer.

### Gate 2 — Findings Sign-Off
Triggered after Stage 3 (EDA). Before any dashboard or visualization work begins.

The analyst must confirm:
- Every finding has been read, not just the summary metric
- Counterintuitive findings have been investigated, not accepted at face value
- Domain knowledge has been applied to validate the direction of numbers (not just their value)
- Any findings that require a client conversation are flagged before delivery

**Gate 2 failure mode:** Building a polished dashboard on findings that contain a silent code logic error. The chart looks correct. The number is wrong.

### Gate 3 — Dashboard Validation
Triggered after Stage 5 (Dashboard Build). Before any client-facing delivery.

The analyst must confirm:
- Every metric on the dashboard traces back to a row in the semantic layer
- Every semantic layer number traces back to the Gate 1-cleared dataset
- The dashboard loads without errors across the target environment
- Labels, filters, and interactive elements behave as intended

**Gate 3 failure mode:** Delivering a dashboard where a KPI card shows a different number than the underlying table because the aggregation logic was applied twice.

---

## Human Judgment Checkpoints

Three categories of decision that Claude Code cannot make and should never be asked to make:

**1. Anomaly disposition**
Claude Code flags. The analyst decides.
A near-duplicate detection that removes 40% of a branch's reviews is not a data quality problem — it might be standard user behavior in that market. Only the analyst with domain knowledge can make that call. Auto-removing based on a threshold will silently corrupt the dataset.

**2. Directional sanity checks**
If the output says "every entity is trending in the same direction," that is a signal to re-read the underlying numbers, not a finding to accept. Uniform direction in messy real-world data is almost always a code logic error. The analyst catches this because domain knowledge creates an expectation the data should violate.

**3. Findings that require a conversation**
Some numbers are correct and still need context before they reach a client. A 90% negative rating in a single quarter for a single location is a fact. Whether it reflects a staffing event, a competitor attack, or a scraping artifact is a judgment call. The analyst flags it for follow-up rather than letting a correct number generate an incorrect conclusion.

---

## Output Structure (Applied Projects)

Each project using this workflow produces the following artifacts:

```
project-name/
├── README.md               # Methodology + findings summary (no raw data)
├── data/
│   ├── raw/                # Original source files (not committed if confidential)
│   └── clean/              # Gate 1-cleared dataset
├── analysis/
│   ├── eda_outputs/        # Gate 2 CSVs — one file per analysis layer
│   └── semantic_layer/     # Gate 3 CSVs — dashboard-ready aggregations
├── dashboard/
│   └── dashboard.html      # Self-contained HTML (no server, no npm, no external APIs)
└── session_recap.md        # Full audit trail: decisions made, gates cleared, open items
```

The session recap is not optional. It is the paper trail that makes the work auditable, reproducible, and transferable to any future analyst who touches the project.

---

## Applied Projects

| Project | Type | Data | Status |
|---------|------|------|--------|
| A/B Testing — Marketing Campaign | Statistical analysis (two-proportion z-test) | Public synthetic dataset | In progress — April 2026 |
| Multi-Location Sentiment Analysis | NLP + operational dashboard | Client data (confidential) | Complete — April 2026 |

---

## Principles

**Prompt architecture is the skill.**
The hours you invest in designing the prompt return as hours you do not spend fixing bad output. A weak prompt produces output that looks correct and is not. A strong prompt produces output that is auditable from the first run.

**Never skip the gates.**
The gates exist because compounding errors are invisible until they reach the client. A wrong row count in Stage 2 becomes a wrong trend line in Stage 3 becomes a wrong KPI card in Stage 5. The gate catches the error where it originates, not where it surfaces.

**Speed without loss of quality.**
Claude Code compresses execution time. The analyst's time goes to judgment, not mechanics. A project that would take three days of manual scripting takes one session with this method — at the same or higher quality, because every transformation is logged and every finding is signed off.

**Document everything.**
The session recap, the cleaning log, the validated findings list. The paper trail is the professionalism signal. It is also what makes your work reviewable by a hiring manager, a client, or a future version of yourself six months later.

---

*Last updated: April 2026*  
*Questions or collaboration: [linkedin.com/in/vadayogi](https://linkedin.com/in/vadayogi)*
