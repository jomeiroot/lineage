# Lineage

**Can you tell where in the world a SARS-CoV-2 sample came from, using only its genome?**

Yes — about 80% of the time, with a model simple enough to read. And if you're not careful about
*how* you build it, the same pipeline will happily report 94% by cheating. Both halves of that
sentence are what this project is about.

📓 **[The whole project is one annotated notebook → `lineage.ipynb`](lineage.ipynb)**

## What's inside

Working from 1,538 complete SARS-CoV-2 genomes (an early-pandemic 2020 snapshot from the
[NCBI Virus database](https://www.ncbi.nlm.nih.gov/labs/virus/vssi/#/), aligned to the Wuhan-Hu-1
reference), the notebook:

- **Builds an interpretable classifier** — each recurrent substitution (e.g. `A23403G`) becomes a
  feature; a multinomial logistic regression assigns genomes to Asia, North America, or Oceania.
  **79.5% held-out accuracy, 79.5% ± 3.6% across 5-fold CV** (random baseline: 33%).
- **Reads the model's reasoning** — the top mutations per region, mapped onto the SARS-CoV-2 gene
  map (ORF1ab, spike, UTRs), including why the famous spike **D614G** mutation — present in over
  half the genomes — is nearly useless for telling regions apart.
- **Demonstrates the trap** — a naive featurization (sequencing gaps and singletons included, no
  deduplication) scores **94%**, and the notebook shows exactly where that number comes from: the
  model fingerprints *sequencing labs*, not viral evolution. A worked example of batch effects and
  train/test leakage.

## Reproduce it

The dataset ships with the repo (406 KB), the run is seeded, and the only dependencies are numpy,
pandas, scikit-learn, and matplotlib:

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace lineage.ipynb   # ~20 seconds
```

Or open it in Colab (the notebook fetches the data itself):
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jomeiroot/lineage/blob/main/lineage.ipynb)

## Layout

```
lineage.ipynb            the project: pipeline, results, analysis
data/
  cov2_sequences.fasta.gz  1,538 aligned genomes (NCBI Virus, 2020 snapshot)
requirements.txt
```

---

*This project began as a mentored high-school project during the first pandemic winter; I rebuilt it
from scratch in 2026 — which is when I discovered that my original 92% was really an 80% wearing a
batch effect. Sequence data is public NCBI data.*
