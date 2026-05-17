# DSA 210 — Final Report
## Analysis of Instagram Content Types and Engagement

**Student:** Duru Doğan — 35759  
**Course:** DSA 210 Introduction to Data Science, Spring 2026  
**Submission Date:** May 2026

---

## 1. Motivation

Social media platforms, especially Instagram, have become central to digital marketing, personal branding, and communication. As different content formats — photos, reels, and carousels — grow in prevalence, understanding which types generate higher engagement has become increasingly valuable for creators and brands alike.

Beyond format, *when* content is posted may also matter. User populations have predictable online activity windows based on daily routines (school, work, leisure), and posting during high-traffic periods could amplify reach.

This project investigates two related questions:

1. Does content type (image, carousel, reel) significantly affect engagement?
2. Do posts shared during peak user activity hours receive higher engagement than off-peak posts?

The goal is not only to answer these questions statistically, but also to apply a rigorous and methodologically sound data science pipeline — including an honest assessment of where the data and models fall short.

---

## 2. Data Sources

### Primary Dataset — Instagram Analytics

| Property | Detail |
|---|---|
| Source | Kaggle — Instagram Reach Analysis |
| Size | ~29,000 posts |
| Key columns | `media_type`, `likes`, `comments`, `engagement_rate`, `post_hour`, `performance_bucket_label`, `follower_count`, `caption_length`, `hashtags_count`, `content_category`, `account_type`, `has_call_to_action`, `day_of_week` |

### Enrichment Dataset — Social Media & Mental Health Survey (HybridDataset)

| Property | Detail |
|---|---|
| Source | Kaggle — Social Media and Mental Health |
| Size | ~480 survey responses |
| Key columns | Occupation, daily online hours, primary platform used |

The two datasets do **not** share common user identifiers and are **not directly merged**. The HybridDataset serves a specific methodological role: providing an independent behavioral signal to define peak hours — this is explained fully in Section 4.

---

## 3. Data Preparation

### 3.1 Quality Check

The Instagram Analytics dataset was inspected for missing values and duplicates. No significant data quality issues were found. The three content types (image, carousel, reel) were approximately equally represented, which is favorable for balanced hypothesis testing.

### 3.2 Engagement Metric

A single combined engagement score was constructed:

```
engagement = likes + comments
```

This captures both passive appreciation (likes) and active interaction (comments) in one interpretable count. The distribution of this metric is heavily right-skewed (skewness > 1), with a mean of 296 and a median of 205 — confirming that a small number of viral posts pull the mean upward. This skew directly motivates the use of non-parametric statistical tests.

### 3.3 Peak Hour Definition — Methodological Design Choice

**This is the most critical design decision in the project, and the one revised in response to instructor feedback.**

**Original (flawed) approach:** Peak hours were originally defined as hours where the average `engagement_rate` in the Instagram dataset exceeded the 67th percentile. The peak/off-peak label was then tested against the same engagement data. This is **circular reasoning** — the label was derived from the outcome variable, so the test was almost guaranteed to find a significant result regardless of whether a true effect existed.

**Revised approach:** Peak hours are defined entirely from the **HybridDataset survey**, an independent data source that captures *when the user population is online*, not their engagement levels. The survey shows:

- 84.8% of respondents are students; 14.2% are working professionals
- 76.1% spend 2–8 hours online daily
- Instagram is the primary platform for 44.2% of respondents

Given this demographic profile, peak internet activity naturally clusters around three daily windows consistent with student and professional schedules:

| Window | Hours | Rationale |
|---|---|---|
| Morning | 08:00–10:00 | Before class / commute; checking feeds on waking |
| Midday | 11:00–13:00 | Lunch break; between classes |
| Evening | 17:00–22:00 | After school/work; prime leisure time |

Each post in the Instagram Analytics dataset is then labeled **Peak** or **Off-Peak** based solely on its `post_hour` column — a variable independent of `engagement`. This eliminates the circularity and makes the H2 test methodologically valid.

---

## 4. Exploratory Data Analysis

### 4.1 Engagement Distribution

The engagement distribution (likes + comments) is strongly right-skewed. The vast majority of posts receive modest engagement (below 500), while a small fraction of viral posts reaches into the thousands. The log-scale histogram confirms this power-law-like tail. This pattern is characteristic of social media engagement and directly justifies the use of non-parametric tests, which do not assume normality.

### 4.2 Engagement by Content Type

Visual inspection (boxplots and median bar charts) shows that image, carousel, and reel posts have **nearly identical** engagement distributions. The medians are close, the interquartile ranges overlap almost entirely, and the overall shapes are indistinguishable. This visual similarity foreshadows the hypothesis test result for H1.

### 4.3 Peak vs. Off-Peak Engagement

After applying the survey-derived peak hour labels, peak-hour and off-peak-hour posts show very similar engagement distributions. The boxplots and mean bar charts (using `engagement_rate`) are nearly indistinguishable between the two groups — a strong visual signal that there is no substantial timing effect in this data after the circularity is corrected.

### 4.4 Content Type × Hour Heatmap

The heatmap of median engagement rate across content type and posting hour shows no consistent pattern. No content type systematically outperforms others at any hour, and no hour consistently produces higher engagement across content types. The slight variation visible in the heatmap is consistent with random noise rather than a structured signal.

### 4.5 Hourly Engagement Profile

The median engagement by posting hour (shown as a bar chart with survey-defined peak hours highlighted in orange) shows a nearly flat profile across all 24 hours. Peak hours defined from the survey do not correspond to visibly higher engagement in the Instagram data — which is an informative null finding, not a failure.

---

## 5. Hypothesis Testing

### 5.1 Justification for Non-Parametric Tests

Shapiro-Wilk normality tests (on n=500 samples per group) confirmed that engagement is non-normally distributed in every group (p < 0.001 in all cases). Combined with the visible right-skew in the distribution plots, this justifies the use of non-parametric tests throughout.

### 5.2 H1 — Content Type vs. Engagement

**Null hypothesis (H₀):** Median engagement is equal across image, carousel, and reel posts.  
**Alternative (H₁):** At least one pair of content types has a different median engagement.  
**Test:** Kruskal-Wallis, followed by pairwise Mann-Whitney U with Bonferroni correction (adjusted α = 0.0167).

**Result:** The Kruskal-Wallis test **failed to reject H₀** (p ≥ 0.05).

**Interpretation:** There is no statistically significant difference in engagement across the three content types. Image, carousel, and reel posts perform comparably in this dataset. This is consistent with the visual analysis — the engagement distributions were near-identical across content formats.

### 5.3 H2 — Peak-Hour vs. Off-Peak Engagement

**Null hypothesis (H₀):** Median engagement is equal for peak-hour and off-peak-hour posts.  
**Alternative (H₁):** Peak-hour posts have higher median engagement (one-tailed).  
**Test:** Mann-Whitney U, α = 0.05.

**Result:** The test **failed to reject H₀** (p ≥ 0.05).

**Interpretation:** After correcting for the methodological circularity — using survey-derived peak hours instead of engagement-derived ones — there is no significant engagement advantage for peak-hour posts. The visual difference observed with the original (circular) definition was an artifact of the circular label construction, not a true signal in the data. This null finding is arguably the most important methodological lesson from this project.

---

## 6. Machine Learning Phase

### 6.1 Objective

The ML phase attempts to predict an Instagram post's **performance category** (low / medium / high / viral) from features available at or before the time of posting.

### 6.2 Target Variable

The dataset contains a pre-existing four-class label `performance_bucket_label` with a perfectly balanced distribution (~7,500 posts per class). With four equally represented classes, the random baseline accuracy is exactly **25%**.

### 6.3 Features

Only features that are causally prior to or independent of the outcome were used — no post-hoc metrics such as `reach`, `impressions`, or `saves` (which are consequences of engagement, not causes):

| Feature | Type |
|---|---|
| `media_type` | categorical |
| `account_type` | categorical |
| `content_category` | categorical |
| `follower_count` | numeric |
| `post_hour` | numeric |
| `day_of_week` | categorical |
| `activity_period` (Peak/Off-Peak) | categorical (survey-derived) |
| `has_call_to_action` | binary |
| `caption_length` | numeric |
| `hashtags_count` | numeric |

### 6.4 Models and Evaluation

Four classifiers were trained with 5-fold stratified cross-validation and evaluated on a 20% held-out test set:

| Model | CV Accuracy | Test Accuracy |
|---|---|---|
| Logistic Regression | 0.245 ± 0.003 | 0.251 |
| Decision Tree | 0.248 ± 0.003 | 0.254 |
| **Random Forest** | **0.258 ± 0.004** | **0.250** |
| Gradient Boosting | 0.249 ± 0.003 | 0.248 |
| Random baseline | — | 0.250 |

All four models perform at or near the random baseline of 25%. This is not a sign of implementation error — it is a meaningful finding.

### 6.5 Feature Importance (Random Forest)

The Random Forest feature importance analysis (Mean Decrease in Gini Impurity) ranked features as follows:

1. **Caption Length** (0.2017) — highest importance
2. Follower Count (0.1564)
3. Posting Hour (0.1444)
4. Hashtag Count (0.1423)
5. Content Category (0.1192)
6. Day of Week (0.1018)
7. Media Type (0.0546)
8. Account Type (0.0297)
9. Has Call-to-Action (0.0287)
10. **Activity Period / Peak-Off-Peak (0.0212)** — lowest importance

Even the highest-ranked feature (Caption Length) contributes only ~20% importance, and no single feature dominates. Importantly, `media_type` and `activity_period` — the two features directly related to H1 and H2 — rank near the bottom.

### 6.6 Confusion Matrix (Decision Tree — Best by Test Accuracy)

The confusion matrix for the Decision Tree model (test accuracy 25.4%) shows a striking pattern: the model consistently predicts "low" for nearly all instances regardless of the true class. This is a classic symptom of a model that has found a dominant-frequency shortcut rather than genuinely learning to discriminate between classes — even though the classes are balanced. It confirms that the features do not carry enough signal to distinguish performance categories.

---

## 7. Findings

**H1 — Content Type Effect:** Not supported. Image, carousel, and reel posts do not produce significantly different engagement levels. Content format alone is not a reliable predictor of performance.

**H2 — Peak Hour Effect:** Not supported (after methodological correction). When peak hours are defined using an independent behavioral signal (survey data), there is no significant engagement advantage to posting during peak hours. The previously observed effect was an artifact of circular label construction.

**ML Phase:** All four classifiers perform at the random baseline (~25%). The features available at posting time — format, timing, hashtags, caption length, account type — do not predict engagement category. The feature importance analysis confirms that no single posting strategy feature is a strong predictor of performance category. Engagement outcomes are driven primarily by factors not captured in structured metadata.

**Central takeaway:** Neither content type nor posting timing is a reliable lever for engagement. This is a coherent, interpretable result — not a failed analysis. It suggests that engagement is predominantly driven by content quality, visual appeal, follower network characteristics, and platform-algorithmic amplification — factors that cannot be captured as structured metadata features.

---

## 8. Limitations and Future Work

### Limitations

**Dataset quality:** The perfectly balanced four-class distribution in `performance_bucket_label` (~7,500 per class) is highly unusual for real Instagram data. This suggests the dataset may have been synthetically generated, stratified, or heavily preprocessed. A synthetic dataset may not accurately reflect the dynamics of organic Instagram engagement.

**Missing confounders:** The most important predictors of engagement are absent from the dataset — content visual quality, caption sentiment and tone, follower network structure and activity, audience demographics, and the timing and extent of algorithmic promotion. No structured metadata feature set can fully capture these.

**HybridDataset generalizability:** The survey sample (n ≈ 480) is small and skewed toward students. Peak hour definitions derived from this sample may not generalize to the full Instagram user population.

**Temporal effects:** The dataset is treated as cross-sectional. Trends in content format preferences and user behavior change over time, and no longitudinal analysis was conducted.

### Future Work

**Richer text features:** Applying NLP to post captions — sentiment analysis, readability scoring, topic modeling — could add meaningful predictive signal.

**Image content features:** Computer vision embeddings of post images would capture visual quality, which is likely the dominant engagement driver.

**Regression over classification:** Predicting the raw engagement count (or log-transformed count) via regression rather than discretized buckets may be a more informative modeling target.

**Better survey data:** A larger, more representative online behavior survey would produce more reliable peak-hour definitions and demographic context.

**Longitudinal analysis:** Tracking how format preferences and engagement patterns shift across time (e.g., the rise of Reels) would add important context.

---

## 9. AI Tool Usage Disclosure

In accordance with the course academic integrity requirements, the use of AI tools is disclosed here:

Claude (Anthropic) was used to assist with:
- Structuring and refining notebook markdown cells and section headings
- Drafting and editing the final report prose
- Reviewing the methodological circularity issue and formulating the corrected peak-hour definition

All analysis code, statistical test choices, and interpretations were authored by the student. AI-generated prose was reviewed, edited, and approved by the student before inclusion.

---

## 10. Repository Structure

```
repo/
├── data/
│   ├── Instagram_Analytics.csv
│   └── HybridDataset.csv
├── notebooks/
│   ├── EDA_and_Hypothesis_Tests.ipynb
│   └── ML_Phase.ipynb
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

*DSA 210 — Introduction to Data Science, Sabancı University, Spring 2026*
