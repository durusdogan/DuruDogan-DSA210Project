# DSA 210 Project  
## Analysis of Instagram Content Types and Engagement  

---

## Research Question  
Does content type significantly affect engagement on Instagram, and how is this relationship influenced by user activity levels?

---

## Overview  
This project investigates how different types of Instagram content influence user engagement and whether posting time plays a role in this relationship.

With the rapid growth of social media platforms, Instagram has become one of the most important tools for communication, marketing, and content creation. Different content formats such as photos, reels, and carousels are widely used, yet their effectiveness in generating engagement may vary.

This project analyzes engagement at the post level and enriches the analysis with general user activity patterns derived from a secondary dataset.

---

## Terminology  

- **Engagement:** Total interaction on a post, calculated as:  
  **Engagement = Likes + Comments**

- **Post Type:** Type of Instagram content (photo, reel, carousel)  
- **Timestamp:** Date and time when a post was shared  
- **Posting Hour:** Extracted hour from timestamp  
- **Peak Hours:** Time periods when user activity is highest  
- **Off-Peak Hours:** Time periods with relatively lower activity  
- **Activity Level:** Indicates whether a post was shared during peak or off-peak hours  

---

## Motivation  
Social media platforms, especially Instagram, play a central role in digital marketing and communication. As content formats diversify, understanding which types of posts generate higher engagement has become increasingly important.

Additionally, user activity varies throughout the day, which may influence how content performs. This project combines content-related factors with temporal activity patterns to better understand engagement dynamics.

---

## Hypotheses  

- **H₀ (Null Hypothesis):**  
  There is no significant difference in mean engagement across content types or activity levels.

- **H1:**  
  Mean engagement differs significantly across content types (photo, reel, carousel).

- **H2:**  
  Posts shared during peak activity hours have higher mean engagement than those shared during off-peak hours.

- **H3:**  
  There is a statistically significant interaction effect between content type and posting time on engagement.

---

## Data Sources  

- **Primary Dataset (Kaggle):**  
  Instagram Post Dataset (~29,000 posts)  
  Includes: likes, comments, post type, timestamp  
  👉 Link: [ADD LINK HERE]

- **Secondary Dataset (Enrichment):**  
  Instagram User Activity Dataset  
  Includes: activity patterns, time-based usage trends  
  👉 Link: [ADD LINK HERE]

---

## Data Preparation and Enrichment  

The main dataset will be cleaned and processed to ensure consistency.

### Feature Engineering:
- Engagement = likes + comments  
- Posting hour extracted from timestamp  

### Enrichment Strategy:
Since the datasets do not share common identifiers, they will not be directly merged.

Instead, the secondary dataset will be used to calculate **average activity levels for each hour of the day**. Based on these values:

- High-activity hours → labeled as **Peak**
- Low-activity hours → labeled as **Off-Peak**

Each post in the primary dataset will then be assigned an **activity level label** based on its posting hour.

---

## Analysis Plan  

### 1. Exploratory Data Analysis (EDA)
- Distribution of engagement  
- Engagement across content types  
- Engagement across posting hours  

### 2. Statistical Testing  

- **One-way ANOVA:**  
  To test differences in mean engagement across content types (H1)

- **Independent Samples t-test:**  
  To compare engagement between peak and off-peak hours (H2)

- **Two-way ANOVA:**  
  To analyze interaction effects between content type and activity level (H3)

### 3. Visualization  
- Boxplots of engagement by content type  
- Line plots of hourly engagement trends  
- Interaction plots (content type × activity level)  

---

## Expected Outcome  
This project is expected to show that engagement varies across content types and that posting during peak activity periods improves performance.

It may also reveal that certain content types perform better depending on when they are shared.

---

## Limitations  
- No direct user matching between datasets  
- Behavioral patterns are generalized  
- Results depend on dataset scope  

---

Duru Doğan - 35759
