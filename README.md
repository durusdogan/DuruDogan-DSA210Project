# DSA 210 — Analysis of Instagram Content Types and Engagement

**Student:** Duru Doğan — 35759  
**Course:** DSA 210 Introduction to Data Science, Spring 2026

---

## Research Question

Does content type significantly affect engagement on Instagram, and is this effect influenced by user activity levels (peak vs. off-peak posting hours)?

---

## Hypotheses

**H1:** Different Instagram content types (image, carousel, reel) lead to significantly different engagement levels.
- Test: Kruskal-Wallis, then pairwise Mann-Whitney U with Bonferroni correction (α = 0.05/3 = 0.0167)
- Result: **H₀ not rejected** — no significant difference across content types

**H2:** Posts shared during peak user activity hours receive higher engagement than off-peak posts.
- Test: Mann-Whitney U, one-tailed (α = 0.05)
- Result: **H₀ not rejected** — no significant timing effect (after methodological correction)

**Engagement metric:** `engagement = likes + comments`

---

## Methodological Note — Peak Hour Definition

**Original problem (instructor feedback):** Peak hours were originally defined as hours where average `engagement_rate` in the Instagram dataset exceeded the 67th percentile — then engagement was tested against those same labels. This is **circular reasoning**: the label was derived from the outcome variable, biasing the test toward finding significance.

**Fix applied:** Peak hours are now defined entirely from the **HybridDataset survey** — an independent source capturing *when the user population is online*, not their engagement levels. Survey demographics (84.8% students, 76.1% spending 2–8 hours online, Instagram as primary platform for 44.2%) motivate three behavioral windows:

| Window | Hours |
|---|---|
| Morning | 08:00–10:00 |
| Midday | 11:00–13:00 |
| Evening | 17:00–22:00 |

The `post_hour` column (independent of engagement) is used for labeling. Circularity eliminated.

---

## Datasets

| Dataset | Source | Description |
|---|---|---|
| Instagram Analytics | [Kaggle – Instagram Reach Analysis](https://www.kaggle.com/datasets/instagram-reach-analysis) | ~29,000 posts with likes, comments, post type, timestamps, follower count, etc. |
| HybridDataset | [Kaggle – Social Media & Mental Health](https://www.kaggle.com/datasets/souvikahmed071/social-media-and-mental-health) | ~480 survey responses on user behavior and platform preferences |

---

## Key Findings

| Finding | Detail |
|---|---|
| H1 (Content Type) | Not supported — image, carousel, reel show near-identical engagement distributions |
| H2 (Posting Time) | Not supported — no significant engagement advantage for peak-hour posts after correcting for circularity |
| ML accuracy | All models perform at ~25% (random baseline for balanced 4-class problem) |
| Most important feature | Caption Length (0.2017 Gini importance in Random Forest) |
| Least important | Activity Period / Peak-Off-Peak (0.0212) |

**Central interpretation:** Engagement is driven by factors not captured in structured metadata — content quality, visual appeal, follower network, and algorithmic amplification. Posting strategy variables (format, timing, hashtags) have negligible predictive power.

---

## Repository Structure

```
repo/
├── data/
│   ├── Instagram_Analytics.csv
│   └── HybridDataset.csv
├── notebooks/
│   ├── EDA_and_Hypothesis_Tests.ipynb    ← EDA + hypothesis tests (with methodological fix)
│   └── ML_Phase.ipynb                    ← ML: 4 classifiers, feature importance
├── figures/
│   ├── engagement_distribution.png
│   ├── engagement_by_type.png
│   ├── peak_vs_offpeak.png
│   ├── heatmap.png
│   ├── hourly_engagement_profile.png
│   ├── model_comparison.png
│   ├── confusion_matrix.png
│   └── feature_importance.png
├── requirements.txt
├── README.md
└── FINAL_REPORT.md
```

---

## How to Run

```bash
pip install -r requirements.txt
mkdir -p figures

# Run EDA and hypothesis tests
jupyter notebook notebooks/EDA_and_Hypothesis_Tests.ipynb

# Run ML phase
jupyter notebook notebooks/ML_Phase.ipynb
```

Run all cells with **Cell → Run All** in each notebook.

---

## Requirements

See `requirements.txt`. Main dependencies: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `scikit-learn`, `jupyter`.

---

## AI Tool Usage

Claude (Anthropic) was used to assist with structuring notebook markdown, drafting the final report, and reviewing the methodological circularity issue. All analysis code and interpretations were authored by the student. Per course requirements, all AI assistance is disclosed here.
