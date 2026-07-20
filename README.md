# Clinical NLP Model Comparison (MSc Dissertation)

Comparing a transformer model against a classical NLP pipeline for extracting diseases and
treatments from clinical notes.

## Problem
Hospitals hold vast amounts of free-text clinical notes. Automatically extracting entities such as
diseases and treatments unlocks research and analytics, but which tool works best on messy,
domain-specific medical text? The common assumption is that large transformer models win; this
project tested that assumption directly.

## Data
40,000+ clinical notes from MIMIC-IV, a large public critical-care dataset.

## Approach
- Evaluated two approaches for clinical entity extraction: **ClinicalBERT** (transformer) and
  **Stanza** (classical NLP pipeline), with **SciSpaCy** in the toolkit.
- Measured **precision and recall** for two entity types: Disease and Treatment.

## Result
Stanza outperformed ClinicalBERT on both entity types:

| Entity | Stanza (Precision / Recall) | ClinicalBERT (Precision / Recall) |
|---|---|---|
| Disease | 23.1% / 23.8% | 3.5% / 3.7% |
| Treatment | 8.9% / 11.2% | 0.4% / 0.5% |

The classical pipeline beat the transformer on this domain-specific extraction task, showing that
the newest or largest model is not always the right one.

## Business takeaway
Model selection should be driven by evidence on the actual data, not by hype. Choosing the right
tool here meant materially better extraction quality at lower compute cost.

## Tech stack
Python, ClinicalBERT, Stanza, SciSpaCy, MIMIC-IV.
