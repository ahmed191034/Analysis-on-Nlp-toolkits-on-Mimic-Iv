# Clinical NLP Toolkit Comparison: ClinicalBERT vs. Stanza on MIMIC-IV

A comparative evaluation of two open-source clinical NLP pipelines — **ClinicalBERT** (transformer-based) and **Stanza/i2b2** (BiLSTM-CRF) — for entity extraction from ICU discharge summaries in the MIMIC-IV database. Built as a dissertation project; evaluates extraction quality, computational efficiency, and practical deployment feasibility for real clinical text.

**Tools:** Python · PyTorch · HuggingFace Transformers · Stanza (Stanford NLP) · Pandas

---

## Project Overview

Clinical text is messy: abbreviations, inconsistent formatting, and domain-specific language make standard NLP tools unreliable. This project runs two purpose-built clinical NER approaches head-to-head on the same corpus of ICU discharge summaries and measures which one actually performs better in practice:

- **ClinicalBERT** — a transformer model fine-tuned for clinical named entity recognition.
- **Stanza (i2b2)** — a sequence-labelling pipeline (BiLSTM-CRF) trained on the i2b2/VA shared-task data.

The pipeline covers preprocessing and abbreviation expansion, batched inference over a 40,000-note corpus, runtime profiling, and evaluation against manually annotated ground truth.

## Results

Entity-level precision/recall/F1 against the manually annotated ground truth, evaluated under both strict matching (exact span + type match) and lenient matching (partial span overlap counts).

| Model | Entity Type | Strict Precision | Strict Recall | Strict F1 | Lenient Precision | Lenient Recall | Lenient F1 |
|---|---|---|---|---|---|---|---|
| ClinicalBERT | Disease | 0.40% | 0.42% | 0.41% | 3.49% | 3.69% | 3.55% |
| ClinicalBERT | Treatment | 0% | 0% | 0% | 0.40% | 0.51% | 0.45% |
| Stanza (i2b2) | Disease | 0% | 0% | 0% | 23.08% | 23.77% | 23.42% |
| Stanza (i2b2) | Treatment | 0% | 0% | 0% | 8.93% | 11.24% | 9.80% |

**Reading the results:** Stanza clearly outperforms ClinicalBERT on both entity types under lenient matching, though both models score low in absolute terms, and strict-match scores collapse to near zero across the board. That gap between strict and lenient — rather than either model doing well outright — is itself the finding worth highlighting: it points to a span-alignment or tokenization-boundary mismatch between the extraction pipeline's output offsets and the ground-truth annotation offsets, rather than either model genuinely failing to find entities. Stanza's larger lenient-match margin over ClinicalBERT also suggests its sequence-labelling (BiLSTM-CRF) approach is picking up more of the entity span even when exact boundaries don't align, while ClinicalBERT's transformer tokenization appears to be diverging further from the annotation scheme. The practical takeaway: before trusting either model's absolute scores, the tokenization/offset alignment step needs auditing — that's the next concrete step for this project.

## Repository Structure

```
├── src/
│   ├── preprocessing.py           # text cleaning + abbreviation expansion
│   ├── clinicalbert_extraction.py # ClinicalBERT inference
│   ├── stanza_extraction.py       # Stanza/i2b2 inference
│   └── evaluation.py              # precision/recall/F1 against ground truth
├── notebooks/
│   └── main_pipeline.ipynb
├── ClinicalBERT_40k.csv           # ClinicalBERT entity outputs
├── Stanza_40000.csv               # Stanza entity outputs
├── requirements.txt
└── environment.yml
```

## Installation

```bash
conda create -n clinical_ner python=3.10
conda activate clinical_ner
pip install -r requirements.txt

# Optional — ClinicalBERT benefits significantly from GPU acceleration
pip install torch==2.3.0 --index-url https://download.pytorch.org/whl/cu121
```

## Dataset Access (not included in this repo)

This project uses **MIMIC-IV Note Events — Discharge Summaries**, available via [PhysioNet](https://physionet.org/content/mimiciv/) after completing their required credentialing. The dataset is not included here due to licensing, privacy, and size restrictions.

To reproduce:
1. Download `discharge.csv` from MIMIC-IV after credential approval.
2. Update the file path in `src/preprocessing.py`.
3. Run preprocessing: `python src/preprocessing.py`

## Running the Pipeline

```bash
python -m src.run_pipeline
```

Or step through `notebooks/main_pipeline.ipynb` interactively.

**Outputs:**
- `df_40k_cleaned.csv` — preprocessed notes
- `ClinicalBERT_40k.csv` / `Stanza_40000.csv` — entity extraction outputs per model
- Evaluation summary (precision, recall, F1) via `src/evaluation.py`

## System Requirements

| Component | Minimum | Recommended |
|---|---|---|
| Python | 3.9 | 3.10 |
| RAM | 8 GB | 16 GB+ |
| GPU | Optional | CUDA-enabled, for faster ClinicalBERT inference |
| Disk | 10 GB | 20 GB+ for MIMIC-IV subsets |

The pipeline is deterministic aside from sampling (`random_state=42`).

## Third-Party Components

| Component | Source | License |
|---|---|---|
| ClinicalBERT model | HuggingFace Model Hub | Apache-2.0 |
| Transformers library | HuggingFace | Apache-2.0 |
| Stanza Toolkit | Stanford NLP | Apache-2.0 |
| PyTorch | Meta AI | BSD |
| Pandas, NumPy | Community | BSD |

All preprocessing, extraction logic, batching, evaluation, and analysis code was developed by the author for this dissertation.

## Note on AI Assistance

In line with the university's Generative AI policy: ChatGPT was used to improve comment readability, refine academic phrasing, and suggest documentation structure. It was **not** used to generate data, model predictions, ground-truth labels, evaluation metrics, or research conclusions. All AI-assisted text was reviewed and edited by the author.

## Contact

**Muhammad Ahmed Jawaid**
[LinkedIn](https://www.linkedin.com/in/m-ahmed-jawaid-416662253/) · ahmedjawaid513@outlook.com
