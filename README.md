# Cross-Jurisdiction Prediction of Privacy Violation Type and Penalty Severity from Enforcement-Decision Text

Machine learning on real privacy enforcement actions — EU GDPR decisions and US FTC
cases — predicting (1) which violation type / legal provision was at issue and
(2) penalty severity, from the decision text and metadata. Tests whether enforcement
signals transfer across legal regimes and whether transformer models overturn the
Ruohonen & Hjerppe (2020) finding that metadata predicts outcomes better than text.

CSCI 7890 · Georgia Southern University · advised by Dr. Vijayalakshmi Ramasamy

Paper repo (Overleaf-synced): https://github.com/j-recasens/CSCI7890-GDPR-Research-Summer-2026-Nic

## Start here (new researcher)

1. Read the paper draft (`Main.tex` in the paper repo) — it documents every decision.
2. The dataset is `data/unified_cases.csv` (2,754 rows). Built by nb09, enriched by nb11.
3. All ML results in the paper come from `notebooks/12_baselines_unified.ipynb`.
4. Every notebook has a header cell stating its purpose, inputs, outputs, and gotchas.
   nb09 and nb11 headers carry **rerun-order warnings** — read them before rerunning anything.
5. The main open task is Legal-BERT fine-tuning on the unified corpus (needs GPU/Colab).
   Working starter code for embeddings and 512-token truncation is in nb05.

## Pipeline

```
nb02 → nb03 → nb04 ─┐                    (US: scrape → enrich → label)
        nb07 → nb08 ─┤→ nb09 → nb11 → nb12
nb06 ────────────────┘                    (EU: GDPRhub API)
```

Not in the pipeline: nb01 (Enforcement Tracker; kept for the future sector join),
nb05 (US-only baselines; kept for its Legal-BERT starter code), nb10 (label reference).

## Repository map
| Path | description |
|---|---|
| `notebooks/01_data_acquisition.ipynb` | Scrapes CMS GDPR Enforcement Tracker (superseded; has sector field) |
| `notebooks/02_ftc_acquisition.ipynb` | Scrapes FTC legal library, filtered to Privacy & Security, 339 cases |
| `notebooks/03_ftc_enrichment.ipynb` | Pulls press-release text + penalty amounts per FTC case |
| `notebooks/04_eda.ipynb` | US EDA + statute-label recovery (produces `ftc_labeled.csv` — still live) |
| `notebooks/05_baseline_models.ipynb` | US-only baselines (superseded) + Legal-BERT embedding starter code |
| `notebooks/06_GDPR_acquisition.ipynb` | GDPRhub via MediaWiki API to EU decision table |
| `notebooks/07_ftc_labels.ipynb` | Rafal FTC Provisions Library snapshot to US label source |
| `notebooks/08_us_case_table.ipynb` | Merges scrape + labels into operative US table, with audit |
| `notebooks/09_unified_schema.ipynb` | Merges US + EU into the 11-field unified schema |
| `notebooks/10_label_overview.ipynb` | Label structure reference (exploratory) |
| `notebooks/11_unified_eda.ipynb` | Severity tiers (k-means), coarse taxonomy, currency normalization |
| `notebooks/12_baselines_unified.ipynb` | **All paper results**: TF-IDF baselines, transfer, Ruohonen challenge |
| `data/unified_cases.csv` | **2,754 cases, the modeling dataset** (11 fields + penalty_usd, severity_tier, coarse_label) |
| `data/gdprhub_dpa_decisions.csv` | 2,417 EU DPA decisions with text, articles, fines |
| `data/us_cases.csv` | 339 US cases with text, labels, penalties (operative US table) |
| `data/enforcement_tracker_clean.csv` | 3,195 EU fines, metadata only (sector join source) |
| `data/ftc...csv` (others) | Intermediate files, see notebook headers |
| `literature/papers/` | Ruohonen & Hjerppe (2020), Orlando & Santoro (2025) PDFs |
| `literature/scope-doc-annotated-2026-07-29.pdf` | The project scope doc with all annotation rounds (green = advisor, red/purple = researcher iterations, blue = final status). The living version is on the researcher's OneDrive |

## Headline results (nb12, TF-IDF + logistic regression)
| Task | In-dist | EU→US | US→EU |
|---|---|---|---|
| Violation type (4-class) | 0.75 | 0.23 | 0.05 |
| Severity tier (3-class) | 0.55 | 0.56 | 0.13 |

Macro F1. Transfer collapse motivates Legal-BERT. Ruohonen challenge: text-only 0.55
beats metadata-only 0.46 (caveat: our metadata is only jurisdiction + year).

## Open tasks (rough priority)
1. Legal-BERT fine-tuning on unified corpus (Colab/GPU) — the paper's main open claim
2. Add GDPR article dummies to the metadata baseline (exact Ruohonen comparison)
3. Sector join: Enforcement Tracker → GDPRhub cases (authority + date + fine match)
4. Mine the 2,522 FTC case PDFs (`data/ftc_pdf_links.csv`) for true decision texts
5. CCPA / state-level enforcement collection (ask Dr. Ramasamy)

## Environment
Python 3.14 venv (`~/privacy-env`): playwright, pandas, jupyter, scikit-learn, matplotlib, transformers. `data/raw/` is gitignored due to size (165MB); can be regenerated via NB06.
