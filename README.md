# DSA 210 – Analysis of Instagram Content Types and Engagement

**Student:** Duru Doğan – 35759  
**Course:** DSA 210 Introduction to Data Science, Spring 2026

## Research Question

Does content type significantly affect engagement on Instagram, and is this effect influenced by user activity levels (peak vs. off-peak posting hours)?

## Hypothesis

**H1:** Different Instagram content types (image, carousel, reel) lead to significantly different engagement levels.  
- Formally: The median engagement rate differs across at least one pair of content types.  
- Test: Kruskal-Wallis test, followed by pairwise Mann-Whitney U tests with Bonferroni correction (α = 0.05).

**H2:** Posts shared during peak user activity hours receive higher engagement than posts shared during off-peak hours.  
- Formally: The median engagement rate of peak-hour posts is significantly higher than off-peak-hour posts.  
- Test: Mann-Whitney U test (α = 0.05).

**Engagement metric:** `engagement = likes + comments` (a combined count that captures both passive appreciation and active interaction).

## Datasets

| Dataset | Source | Description |
|---|---|---|
| Instagram Analytics | [Kaggle – Instagram Reach Analysis](https://www.kaggle.com/datasets/instagram-reach-analysis) | ~29,000 Instagram posts with likes, comments, post type, and timestamps |
| HybridDataset | [Kaggle – Social Media & Mental Health](https://www.kaggle.com/datasets/souvikahmed071/social-media-and-mental-health) | ~480 survey responses on user behavior, daily usage hours, and platform preferences |

### How the second dataset is used

The two datasets do not share common user identifiers and are **not directly merged**. The HybridDataset is used to derive general Instagram activity patterns:

1. The survey shows the majority of respondents are students who spend 4+ hours online daily and use Instagram as their primary platform.
2. This justifies using engagement-based peak hour detection on the main dataset: hours where the average engagement rate falls in the top 33rd percentile are labeled **Peak**, the rest **Off-Peak**.
3. Each post in the Instagram Analytics dataset is then labeled accordingly.

## Repository Structure

```
your-repo/
├── data/
│   ├── Instagram_Analytics.csv
│   └── HybridDataset.csv
├── notebooks/
│   └── EDA_and_Hypothesis_Tests.ipynb
├── figures/
│   ├── engagement_by_type.png
│   ├── peak_vs_offpeak.png
│   └── heatmap.png
├── requirements.txt
└── README.md
```

## How to Run

```bash
pip install -r requirements.txt
mkdir -p figures
jupyter notebook notebooks/EDA_and_Hypothesis_Tests.ipynb
```

Then run all cells (Cell → Run All).

## Requirements

See `requirements.txt`. Main dependencies: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `jupyter`.
