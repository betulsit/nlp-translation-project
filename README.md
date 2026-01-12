# Spanish-to-English Machine Translation Comparison

## Project Overview

This project evaluates and compares the performance of **six state-of-the-art Transformer-based machine translation models** on a **Spanish-to-English** translation task.

All models were evaluated out-of-the-box using pre-trained weights without any task-specific fine-tuning.

---

## Repository Structure

```text
├── notebooks/
│   ├── M2M_100.ipynb        # Meta AI's Many-to-Many model (1.2B)
│   ├── MBart50.ipynb        # mBART-50 multilingual translation
│   ├── MarianMT.ipynb      # Helsinki-NLP pair-specific baseline
│   ├── mT5andFLANT_5.ipynb # Google mT5 and FLAN-T5 experiments
│   └── nllb-200.ipynb      # NLLB-200 distilled model (600M)
├── .gitattributes
├── .gitignore
└── README.md
```

---

## Dataset & Methodology

### Dataset

* **Source:** `Helsinki-NLP/tatoeba_mt`
  (Subset of the Tatoeba Project)
* **Task:** Spanish (Source) → English (Target)
* **Nature:** Human-curated, open-domain sentences containing diverse grammar, vocabulary, and idiomatic expressions

---

### Standardization Protocol (Seed = 42)

To ensure a **scientifically valid and reproducible comparison**, we applied a strict standardization protocol across all notebooks:

1. **Data Inversion**
   The original dataset (`eng-spa`) was structurally inverted to match the `spa → eng` inference task.

2. **Consistent Subsetting**
   A fixed random seed (`seed = 42`) was used to extract the **same 1,000 sentence pairs** for every model.

3. **Evaluation Metrics**
   Models were evaluated using:

   * **BLEU** – n-gram overlap
   * **METEOR** – synonym and morphological matching
   * **BERTScore** – semantic similarity

---

## Models Evaluated

| Model        | Type               | Key Characteristics                                                   |
| ------------ | ------------------ | --------------------------------------------------------------------- |
| **MarianMT** | Pair-Specific      | Baseline optimized for Romance languages; fast and accurate           |
| **NLLB-200** | Multilingual (MoE) | “No Language Left Behind” (Distilled 600M); sparse Mixture-of-Experts |
| **M2M-100**  | Multilingual       | Large-scale many-to-many model supporting 100 languages               |
| **mBART-50** | Multilingual       | Denoising autoencoder pre-trained on 50 languages; strong fluency     |
| **FLAN-T5**  | Instruction-Tuned  | Prioritizes semantic meaning over exact lexical matching              |
| **mT5**      | Multilingual T5    | General-purpose text-to-text model                                    |

---

## Results Summary

Our evaluation shows that **NLLB-200** and **MarianMT** deliver the strongest overall performance, while highly generalist models struggle in a zero-shot translation setting.

| Model        | BLEU  | METEOR | BERTScore (F1) | Performance Notes                                     |
| ------------ | ----- | ------ | -------------- | ----------------------------------------------------- |
| **NLLB-200** | 58.96 | 0.81   | 0.97           | Best balanced performance; high accuracy and fluency  |
| **MarianMT** | 58.89 | 0.74   | 0.98           | Strong baseline; excellent lexical overlap            |
| **M2M-100**  | 49.09 | 0.66   | 0.97           | Good generalization, lower n-gram overlap             |
| **mBART-50** | 44.49 | N/A    | N/A            | Fluent outputs, lower lexical precision               |
| **FLAN-T5**  | ~9.58 | N/A    | N/A            | Low BLEU; strong semantic paraphrasing                |
| **mT5**      | Fail  | Fail   | Fail           | Produced empty or invalid outputs                     |

---

## Key Takeaways

* **Pair-specific models** remain highly competitive for well-defined translation directions.
* **NLLB-200** offers the best trade-off between multilingual flexibility and translation quality.
* **Instruction-tuned models** (FLAN-T5) preserve meaning but sacrifice lexical accuracy.
