This repository for clinical extraction process named Entity_Extraction.ipynb contains all source code developed for a comparative analysis of two open-source NLP toolkits:

ClinicalBERT
Stanza (i2b2)
applied to ICU discharge summaries from the MIMIC-IV database. The project evaluates extraction quality, computational efficiency, and practical deployment feasibility for clinical text processing.

1. Project Overview

The objective of this project is to extract and compare clinically meaningful entities from ICU discharge summaries using two different NLP methods:

ClinicalBERT
A transformer-based model fine-tuned for clinical NER.

Stanza (i2b2)
A sequence-model pipeline (BiLSTM-CRF) trained on the i2b2/VA shared tasks.

The project includes preprocessing, abbreviation expansion, model inference, batching for large-scale processing, runtime profiling, and evaluation against manually annotated ground-truth labels.

2. Code Structure

You may optionally organise your archive as follows:

src/
   preprocessing.py
   clinicalbert_extraction.py
   stanza_extraction.py
   evaluation.py

notebooks/
   main_pipeline.ipynb

df_40k_cleaned.csv (optional intermediate)
ClinicalBERT_40k.csv
Stanza_40000.csv

requirements.txt
environment.yml
README.md


Only small intermediate CSVs may be included if they are under allowed file size limits.

3. Installation Instructions
Pip installation
pip install -r requirements.txt

Conda environment (recommended)
conda create -n clinical_ner python=3.10
conda activate clinical_ner
pip install -r requirements.txt

GPU-enabled installation (optional)

ClinicalBERT benefits significantly from GPU acceleration.

pip install torch==2.3.0 --index-url https://download.pytorch.org/whl/cu121


GPU usage is automatic within the code when available.

4. Dataset Requirements (Not Included)

This project requires access to:
MIMIC-IV Note Events – Discharge Summaries (DS notes)
Available from PhysioNet:
https://physionet.org/content/mimiciv/

Dataset cannot be included due to:

Licensing restrictions.

University submission size limit (maximum 100 MB).

Ethical and privacy considerations.

Recreating the input dataset

Download discharge.csv from MIMIC-IV after credential approval.

Place the file in the expected path or update file paths in the scripts.

Run preprocessing to create the cleaned dataset:

python preprocessing.py

5. Running the Code
Complete pipeline
python untitled64.py

Notebooks

You may run:

notebooks/main_pipeline.ipynb

Outputs

The pipeline generates:

df_40k_cleaned.csv – preprocessed notes.

ClinicalBERT_40k.csv – ClinicalBERT entity outputs.

Stanza_40000.csv – Stanza entity outputs.

Evaluation summaries including precision, recall, F1-scores.

6. Provenance and Use of Third-Party Code

The following open-source tools and models are used:

Component	Source	License
ClinicalBERT model	HuggingFace Model Hub	Apache-2.0
Transformers library	HuggingFace	Apache-2.0
Stanza Toolkit	Stanford NLP	Apache-2.0
PyTorch	Meta AI	BSD
Pandas, NumPy	Community	BSD

All other code (data cleaning functions, entity extraction logic, batching, evaluation, runtime profiling) was developed by the author specifically for this dissertation.

Model loading and pipeline examples follow official documentation conventions.

7. Documentation of AI Use

In compliance with the University’s Generative AI Policy:

Generative AI tools (ChatGPT) were used to assist with:

Improving readability of comments and documentation.

Refining academic phrasing.

Explaining code cells for clarity.

Suggesting organisational structures for the README and project files.

AI tools were not used to:

Generate datasets.

Produce model predictions.

Generate ground truth labels.

Compute evaluation metrics.

Conduct analysis or conclusions of the research.

All AI-assisted text was carefully reviewed and edited by the author.

8. Reproducibility Notes

To reproduce the study:

Install the environment via requirements.txt or environment.yml.

Download MIMIC-IV discharge summaries.

Run preprocessing to clean and normalise the text.

Execute ClinicalBERT and Stanza extraction modules.

Compute performance metrics using evaluation scripts.

The code is deterministic aside from sampling (random_state=42) and may require adequate RAM to handle the 40,000-note corpus. GPU availability improves runtime but is not mandatory.

9. System Requirements
Component	Minimum	Recommended
Python	3.9	3.10
RAM	8 GB	16 GB+
GPU	Optional	CUDA-enabled GPU for faster ClinicalBERT inference
Disk Space	10 GB	20 GB+ for MIMIC-IV subsets
10. Contact Details

For questions related to the code or reproducing results:

Name: Muhammad Ahmed Jawaid
email: ahmedjawaid513@outlook.com
