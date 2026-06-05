<div align="center">

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:0d1117,100:1a1f2e&height=120&text=LLM%20Commercial%20Video%20Reliability&fontSize=28&fontColor=00b4d8&desc=ACM%20Web%20Science%20Conference%202026&descColor=90e0ef&descSize=15" />

[![ACM Paper](https://img.shields.io/badge/ACM%20Published-WebSci%202026%20pp.97–107-ff6b6b?style=for-the-badge&logo=acm&logoColor=white)](https://dl.acm.org/doi/10.1145/3795766.3799760)
[![Python](https://img.shields.io/badge/Python-3.10+-00b4d8?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Gemini](https://img.shields.io/badge/Google%20Gemini%202.5%20Flash-0d1117?style=for-the-badge&logo=google&logoColor=90e0ef)](https://deepmind.google/gemini)
![Status](https://img.shields.io/badge/Status-Published-brightgreen?style=for-the-badge)

</div>

---

## 📌 Overview

Can AI reliably evaluate commercials the way humans do? This research builds a **comprehensive framework for assessing the reliability of Google Gemini 2.5 Flash** in scoring and reasoning about commercial videos — evaluated across two critical dimensions:

| Dimension | Question |
|---|---|
| **Human-AI Agreement** | Do Gemini's scores match trained human annotators? |
| **Score-Reasoning Consistency** | Does Gemini's written reasoning align with its own numeric scores? |

---

## 📦 Dataset

<table>
<tr>
<td align="center"><b>620</b><br/><sub>Super Bowl Commercials<br/>2014–2024</sub></td>
<td align="center"><b>95</b><br/><sub>Annotated Variables<br/>per video</sub></td>
<td align="center"><b>0.93–0.99</b><br/><sub>Human Intercoder<br/>Cohen's Kappa</sub></td>
<td align="center"><b>0.97</b><br/><sub>Human Intercoder<br/>ICC</sub></td>
</tr>
</table>

**Variable Categories:**
- 📢 **Information-Related** — product claims, functional benefits, CTAs (17 variables)
- ❤️ **Emotion-Related** — joy, anger, surprise, nostalgia, humor, drama (37 variables)
- ⭐ **Special Character-Related** — celebrities, babies, animals (25 variables)
- 🏷️ **Brand-Related** — familiarity, quality perception, display time (9 variables)
- 🎵 **Audio-Related** — music popularity, tempo, vocal intensity (7 variables)

---

## ⚙️ Methodology

```
Super Bowl Commercials (620 videos)
            │
            ▼
  ┌─────────────────────┐
  │  Google Gemini 2.5  │  ← Zero-shot prompting
  │  Flash (LMM)        │     across 95 variables
  └──────────┬──────────┘
             │
     ┌───────┴────────┐
     ▼                ▼
Numeric Scores    Free-text Reasoning
(1–5, binary,     Narratives
 quantity)
     │                │
     ▼                ▼
Human-AI         NLP Vectorization
Agreement        ├── TF-IDF
(Kappa, ICC)     └── Sentence Transformers
                      │
                      ▼
               Clustering (KMeans, GMM)
               + LIME Visualization
```

---

## 📊 Key Results

| Variable Type | Human-AI (Kappa/ICC) | Score-Reasoning F1 |
|---|---|---|
| Binary/Objective (e.g., baby present) | Kappa > **0.95** | F1 > **0.90** |
| Subjective scaled (e.g., joy, anger) | Kappa **0.5–0.8** | F1 **0.70–0.90** |
| Actual quantity (e.g., brand duration) | ICC ≥ **0.90** | — |

> **Key finding:** LMM performs reliably on binary/objective variables. For subjective scaled variables, AI scores deviate from human scores by ≤1 rating point. Narrative reasoning for adjacent score levels tends to cluster together, revealing granularity limitations.

---

## 🗂️ Repository Structure

```
llm-commercial-video-reliability/
├── data/
│   └── variable_categories.md
├── prompts/
│   ├── emotion_variables_prompt.json
│   └── brand_variables_prompt.json
├── analysis/
│   ├── human_ai_agreement.py
│   ├── tfidf_clustering.py
│   ├── embedding_clustering.py
│   └── lime_visualization.py
├── results/
│   └── clustering_metrics_table.csv
└── README.md
```

---

## 🛠️ Tools & Libraries

![Python](https://img.shields.io/badge/Python-0d1117?style=flat-square&logo=python&logoColor=00b4d8)
![Gemini](https://img.shields.io/badge/Gemini%20API-0d1117?style=flat-square&logo=google&logoColor=90e0ef)
![Sklearn](https://img.shields.io/badge/Scikit--learn-0d1117?style=flat-square&logo=scikitlearn&logoColor=cae9ff)
![Pandas](https://img.shields.io/badge/Pandas-0d1117?style=flat-square&logo=pandas&logoColor=48cae4)
![NumPy](https://img.shields.io/badge/NumPy-0d1117?style=flat-square&logo=numpy&logoColor=00b4d8)
`Sentence-Transformers` `LIME` `KMeans` `GMM` `TF-IDF` `Matplotlib`

---

## 📎 Citation

```bibtex
@inproceedings{luo2026assessing,
  title     = {Assessing the Reliability of a Large Multimodal Model for
               Scoring and Reasoning Commercial Videos},
  author    = {Luo, Xiao and Fang, Xiang and Wu, Yuechen and
               Chidara, Preethika and Elliott, Rob},
  booktitle = {18th ACM Web Science Conference (WebSci '26)},
  pages     = {97--107},
  year      = {2026},
  doi       = {10.1145/3795766.3799760}
}
```

---

<div align="center">
<sub>Part of <a href="https://github.com/preethikachidara">Preethika Chidara's</a> research portfolio · ACM Web Science 2026</sub>
</div>
