#  DSA 210 Project  
## Analysis of Instagram Content Types and Engagement  

### Does content type significantly affect engagement on Instagram, and how is this relationship influenced by user activity levels?

---

##  Overview  

This project investigates how different types of Instagram content influence user engagement and whether posting time plays a role in this relationship.  

With the rapid growth of social media platforms, Instagram has become one of the most important tools for communication, marketing, and content creation. Different content formats such as photos, reels, and carousels are widely used, yet their effectiveness in generating engagement may vary.  

This project aims to analyze whether certain content types consistently outperform others and whether user activity patterns (e.g., peak vs. off-peak hours) influence engagement outcomes. Rather than focusing on individual users, the analysis is conducted at the post level and enriched with general behavioral patterns derived from user activity data.

---

##  Terminology  

- **Engagement:** Total interaction on a post, calculated as `likes + comments`  
- **Post Type:** Type of Instagram content (`photo`, `reel`, `carousel`)  
- **Timestamp:** Date and time when a post was shared  
- **Posting Hour:** Extracted hour from timestamp  
- **Peak Hours:** Time periods when user activity is highest  
- **Off-Peak Hours:** Time periods with relatively lower activity  
- **Activity Level:** Indicates whether a post was shared during peak or off-peak hours  

---

##  Motivation  

Social media platforms, especially Instagram, play a central role in digital marketing and communication. As content formats diversify, understanding which types of posts generate higher engagement has become increasingly important.  

Additionally, user behavior is not constant throughout the day. Activity levels vary depending on time, and this may directly influence how content performs.  

This project aims to combine content-related factors with user activity patterns to better understand engagement dynamics.

---

##  Research Question  

Does content type significantly affect engagement on Instagram, and is this effect influenced by user activity levels?

---

##  Hypothesis  

- **H₀ (Null Hypothesis):**  
  Content type and posting time have no significant effect on engagement levels.  

- **H₁ (Alternative Hypothesis):**  
  Different Instagram content types lead to significantly different engagement levels, and posts shared during peak activity periods tend to receive higher engagement.  

---

##  Data Sources  

###  Primary Dataset (Kaggle)  
Instagram Post Dataset (~29,000 posts)  

Includes:  
- Likes  
- Comments  
- Post type (photo, reel, carousel)  
- Timestamp  

---

###  Secondary Dataset (Enrichment)  
Instagram User Behavior Dataset  

Includes:  
- Activity patterns  
- Engagement behavior  
- Time-based usage trends  

---

##  Data Preparation  

The main dataset will be cleaned and processed to ensure consistency.  

New features:  
- Engagement (`likes + comments`)  
- Posting hour  
- Activity level (peak vs. off-peak)  

Since datasets do not share user IDs, they will not be directly merged. Instead, behavioral patterns from the enrichment dataset will be used to classify posts based on activity level.

---

##  Analysis Plan  

The project will include:  

- Exploratory Data Analysis (EDA)  
- Visualization of engagement patterns  
- Comparison across content types  
- Comparison between peak and off-peak hours  
- Statistical testing of engagement differences  

The goal is to understand how content type and timing jointly influence engagement.

---

##  Expected Outcome  

This project is expected to show that engagement varies across content types and that posting during peak activity periods improves performance.  

It may also reveal that certain content types perform better depending on when they are shared.

---

##  Limitations  

- No direct user matching between datasets  
- Behavioral patterns are generalized  
- Results depend on dataset scope  

---
