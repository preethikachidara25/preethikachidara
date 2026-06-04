# 🤖 LLM Reliability for Commercial Video Analysis

> **Published at ACM Web Science Conference 2026 (pp. 97–107)**
> 📄 [Read the paper](https://dl.acm.org/doi/10.1145/3795766.3799760)

## Overview

This project presents a comprehensive framework for assessing the reliability of a Large Multimodal Model (LMM) — specifically Google Gemini 2.5 Flash — in evaluating commercial videos at scale.

As AI platforms increasingly use LMMs to generate scores, summaries, and insights for advertisers, a critical question emerges: **can we trust AI-generated evaluations?** This research evaluates that question across two dimensions:

1. **Human-AI Agreement** — Do AI-generated scores align with human annotator scores?
2. **Score-Reasoning Consistency** — Do the AI's numeric scores internally align with its own free-text reasoning?

---

## Dataset

- **620 Super Bowl commercials** spanning 2014–2024
- **95 advertising variables** across 5 categories:
  - Information-Related (product claims, functional benefits, CTAs)
  - Emotion-Related (joy, anger, surprise, nostalgia, humor, etc.)
  - Special Character-Related (celebrities, babies, animals)
  - Brand-Related (familiarity, perceived quality, brand display time)
  - Audio-Related (music popularity, tempo, vocal intensity)
- Human annotations by trained research assistants (Cohen's Kappa: 0.93–0.99; ICC: 0.97)

---

## Methodology

```
Super Bowl Videos (620)
        │
        ▼
Google Gemini 2.5 Flash (Zero-Shot Prompting)
        │
        ├──► Numeric Scores (1–5 scaled, binary, actual quantity)
        │
        └──► Free-text Reasoning Narratives
                │
                ├──► Human-AI Agreement (Cohen's Kappa, ICC)
                │
                └──► Score-Reasoning NLP Analysis
                            │
                            ├── TF-IDF Vectorization
                            ├── Sentence Transformer Embeddings
                            └── Clustering (KMeans, GMM) + LIME
```

### Key Techniques
- **Zero-shot prompting** of Google Gemini across 95 structured variables
- **Cohen's Kappa** for categorical variable agreement; **ICC** for continuous variables
- **TF-IDF and sentence transformer embeddings** to represent AI reasoning narratives
- **KMeans and Gaussian Mixture Model (GMM)** clustering with cosine distance
- **LIME visualizations** to interpret misalignment between scores and narratives

---

## Key Findings

| Variable Type | Human-AI Agreement | Score-Reasoning Consistency |
|---|---|---|
| Binary/Objective (e.g., "Is baby present?") | Cohen's Kappa > 0.95 | F1 > 0.90 |
| Subjective scaled (e.g., joy, surprise, drama) | Kappa 0.5–0.8 | F1 0.70–0.90 |
| Actual quantity (e.g., brand display duration) | ICC ≥ 0.90 | — |

- LMM performs **reliably on binary and objective variables**
- For **subjective scaled variables**, AI scores deviate from human scores by typically ≤1 rating point
- Narrative reasoning for adjacent score levels (e.g., score 2 vs 3) tends to cluster together, revealing **LMM reasoning granularity limitations**

---

## Repository Structure

```
llm-commercial-video-reliability/
│
├── data/
│   └── variable_categories.md       # Descriptions of 95 annotated variables
│
├── prompts/
│   ├── emotion_variables_prompt.json
│   └── brand_variables_prompt.json  # Prompt templates used with Gemini
│
├── analysis/
│   ├── human_ai_agreement.py        # Cohen's Kappa and ICC computation
│   ├── tfidf_clustering.py          # TF-IDF vectorization + KMeans/GMM
│   ├── embedding_clustering.py      # Sentence transformer embeddings + clustering
│   └── lime_visualization.py        # LIME word-weight visualizations
│
├── results/
│   └── clustering_metrics_table.csv # ARI, NMI, Precision, Recall, F1 per variable
│
└── README.md
```

---

## Citation

```bibtex
@inproceedings{luo2026assessing,
  title={Assessing the Reliability of a Large Multimodal Model for Scoring and Reasoning Commercial Videos},
  author={Luo, Xiao and Fang, Xiang and Wu, Yuechen and Chidara, Preethika and Elliott, Rob},
  booktitle={18th ACM Web Science Conference (WebSci '26)},
  pages={97--107},
  year={2026},
  doi={10.1145/3795766.3799760}
}
```

---

## Tools & Libraries

`Python` `Google Gemini API` `Scikit-learn` `Sentence-Transformers` `LIME` `Pandas` `NumPy` `Matplotlib`
