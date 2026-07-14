# Cross-Jurisdiction Prediction of Privacy Violation Type and Penalty Severity from Enforcement-Decision Text

Machine learning on real privacy enforcement actions — EU GDPR decisions and US FTC
cases — predicting (1) which violation type / legal provision was at issue and
(2) penalty severity, from the decision text and metadata. Tests whether enforcement
signals transfer across legal regimes and whether transformer models overturn the
Ruohonen & Hjerppe (2020) finding that metadata predicts outcomes better than text.

CSCI 7890 · Georgia Southern University · advised by Dr. Vijayalakshmi Ramasamy

## Repository map
| Path | description |
|---|---|
| `notebooks/01_data_acquisition.ipynb` | Scrapes CMS GDPR Enforcement Tracker (EU fine metadata) |
| `notebooks/02_ftc_acquisition.ipynb` | Scrapes FTC legal library, filtered to Privacy & Security, 339 cases |
| `notebooks/03_ftc_enrichment.ipynb` | Pulls press-release text + penalty amounts per FTC case |
| `notebooks/04_eda.ipynb` | EDA, violation taxonomy construction, severity tiers |
| `notebooks/05_baseline_models.ipynb` | Preliminary TF-IDF + Legal-BERT baselines |
| `notebooks/06_GDPR_acquisition.ipynb` | GDPRhub via MediaWiki API to EU decision table |
| `notebooks/07_ftc_labels.ipynb` | Rafal FTC Provisions Library snapshot to US label source |
| `notebooks/08_us_case_table.ipynb` | Merges scrape + labels into operative US table, with audit |
| `data/enforcement_tracker_clean.csv` | 3195 EU fines (metadata only) |
| `data/gdprhub_dpa_decisions.csv` | 2417 EU DPA decisions with text, articles, fines |
| `data/us_cases.csv` | 339 US cases with text, labels, penalties (operative US table) |
| `data/ftc...csv` (others) | Intermediate files, see notebook headers |


## Status
- ✓ EU corpus: 2417 decisions with summary texts, article labels, 1231 fines
- ✓ US corpus: 339 cases with press release texts, validated labels, 147 penalties
- ↺ Unified US+EU schema, label adjudication, model reruns, shared violation taxonomy

## Environment
Python 3.14 venv (`~/privacy-env`): playwright, pandas, jupyter, scikit-learn, matplotlib, transformers. `data/raw/` is gitignored due to size (165MB); can be regenerated via NB06.

