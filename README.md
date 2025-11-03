# ClinicalBERT & Stanza Comparison on MIMIC-IV Notes

This project compares two open-source NLP toolkits — **ClinicalBERT** and **Stanza (i2b2)** — for extracting medical entities such as diseases, treatments, and procedures from MIMIC-IV discharge summaries.

## Features
- Cleans and normalizes medical text (abbreviation expansion, noise removal)
- Runs entity extraction using ClinicalBERT and Stanza
- Computes F1-scores comparing model predictions vs ground truth
- Saves structured results in CSV and Excel

## Usage
```bash
pip install -r requirements.txt
python untitled64.py
