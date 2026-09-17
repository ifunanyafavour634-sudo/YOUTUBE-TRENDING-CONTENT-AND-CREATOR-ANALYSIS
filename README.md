# YOUTUBE-TRENDING-CONTENT-AND-CREATOR-ANALYSIS
#  YOUTUBE CONTENT & CREATOR PERFORMANCE ANALYSIS
<img width="585" height="325" alt="Screenshot 2026-09-17 175119" src="https://github.com/user-attachments/assets/bf555148-9efa-4119-946e-b73d70ce1e3e" />


# TABLE OF CONTENTS

* [BACKGROUND](#background)
* [DATA STRUCTURE](#data-structure)

  * [DATA MODEL](#data-model)
  * [DATA PIPELINE](#data-pipeline)
* [EXECUTIVE SUMMARY](#executive-summary)
* [INSIGHTS DEEP DIVE](#insights-deep-dive)
* [RECOMMENDATIONS](#recommendations)
* [ASSUMPTIONS AND CAVEATS](#assumptions-and-caveats)
* [NEXT STEPS](#next-steps)
* [ABOUT](#about)

## BACKGROUND

Youtube operates in a creator and content-driven environment where understanding creator performance, content discovery, audience engagement, and trending behaviour is important for making informed platform decisions.

For a creator-focused platform, views alone do not provide a complete picture of content performance. A video may generate substantial reach without generating proportionally strong audience interaction, while another may attract a smaller audience but demonstrate much stronger engagement and sustained trending performance.

As a Data Analyst working with Youtube business problems, the objective of this project is to use data to understand:

1. Which creators demonstrate strong audience engagement?
2. Which videos attract the most audience attention?
3. Which content categories perform best?
4. What patterns exist in trending and discovery activity over time?
5. Which creators consistently produce content that trends?
6. How strong is audience engagement relative to content reach?

The analysis focuses on the US YouTube Trending dataset and uses a SQL-first analytical workflow to translate raw daily trending observations into creator, content, category, discovery, and engagement insights.

The project uses Python for data acquisition and preparation, PostgreSQL for the main analytical work, and Power BI for visualization.

## DATA STRUCTURE

### DATA MODEL

The project uses the US portion of the Kaggle Trending YouTube Video Statistics dataset.

The original analytical file contains 40,949 records and 16 columns.

The dataset contains daily observations of videos appearing in the US trending feed.

Key fields include:

1. `video_id`: Unique identifier for each YouTube video.
2. `trending_date`: Date on which the video appeared in the trending dataset.
3. `title`: Video title.
4. `channel_title`: Creator/channel associated with the video.
5. `category_id`: YouTube category identifier.
6. `publish_time`: Original publication timestamp.
7. `tags`: Tags associated with the video.
8. `views`: Recorded video views at the time of the observation.
9. `likes`: Recorded likes.
10. `dislikes`: Recorded dislikes.
11. `comment_count`: Recorded comments.
12. `thumbnail_link`: Video thumbnail URL.
13. `comments_disabled`: Indicates whether comments were disabled.
14. `ratings_disabled`: Indicates whether ratings were disabled.
15. `video_error_or_removed`: Indicates whether the video had an error or had been removed.
16. `country`: Region identifier added during preparation.

The dataset is a daily snapshot dataset rather than a transactional dataset. Consequently, the same `video_id` can occur on multiple `trending_date` values.

This distinction is important because summing views across every daily observation would double-count the same video's cumulative view count.

For creator and content performance analysis, the latest observed record for each video is therefore used.

For trending and discovery analysis, repeated daily observations are intentionally retained because they represent how long content remained in the trending ecosystem.

### DATA PIPELINE

The project followed a three-stage analytical workflow:

**1. Data Acquisition — Kaggle API + Python**

The dataset was downloaded from Kaggle using the Kaggle API and Python.

The project focused on the US dataset, `USvideos.csv`, from the `datasnaek/youtube-new` Trending YouTube Video Statistics dataset.

Python was used at this stage because it provided a reproducible way to acquire and prepare the source data before database analysis.

**2. Data Preparation — Python / Pandas**

Python was used to prepare the dataset before loading it into PostgreSQL.

The preparation process included:

* Removing the unnecessary `description` field.
* Adding a `country` field with the value `US`.
* Converting `trending_date` from the source `YY.DD.MM` representation into a proper date format.
* Converting `publish_time` into a consistent datetime representation.
* Checking missing values.
* Checking duplicate records.
* Standardizing numeric fields used for analysis.
* Validating the resulting row and column structure.

The purpose of this preparation stage was to make the dataset consistent and suitable for relational database analysis.

A final analytical-ready version was produced with one record per `video_id` and `trending_date`, resulting in 40,899 analytical records after duplicate handling.

**3. Analytical Processing — PostgreSQL**

The prepared dataset was then moved into PostgreSQL for the main analysis.

PostgreSQL was deliberately used as the primary analytical engine rather than using Power BI to perform the core analysis.

SQL was used to:

* Validate the dataset.
* Control duplicate observations.
* Select the latest snapshot for each video.
* Aggregate creator performance.
* Analyse content performance.
* Analyse category performance.
* Measure trending duration.
* Analyse creator consistency.
* Calculate engagement rates.
* Rank creators and videos.
* Produce business-ready analytical outputs.

**4. Visualization — Power BI**

Power BI was used after the SQL analysis to communicate the findings visually.

The dashboard focuses on the business questions and presents the SQL-derived metrics through interactive charts, KPI cards, and filtering.

Power BI therefore serves as the visualization and reporting layer rather than the primary analytical engine.

## EXECUTIVE SUMMARY

### OVERVIEW

The analysis covers **40,899 analytical video-day observations**, representing **6,351 unique videos**, **2,207 creators**, and **16 content categories** across **205 observed trending dates**.

Because the dataset contains repeated daily snapshots, creator and video performance were evaluated using the latest available observation for each unique video rather than summing historical snapshots.

Across those latest video observations, the dataset contains approximately:

* **12.46 billion views**
* **352.96 million likes**
* **40.97 million comments**
* **3.16% overall engagement rate**

The analysis also shows that reach and engagement represent different dimensions of content performance.

Music generated approximately **4.83 billion latest-snapshot views**, making it the largest category by reach, while Comedy and Howto & Style showed engagement rates above 4% among categories with sufficient volume.

At creator level, ibighit generated approximately **271.75 million latest-snapshot views across nine videos**, while creators such as 5SOSVEVO, ShawnMendesVEVO, and BANGTANTV demonstrated particularly strong aggregate engagement among creators with at least three observed videos.

Trending persistence also varies considerably. Several individual videos remained in the trending dataset for more than four weeks, demonstrating that sustained discovery is a separate performance dimension from total reach.

The analysis therefore evaluates AICines-style creator performance through multiple dimensions rather than relying on a single popularity metric.

## INSIGHTS DEEP DIVE

### CREATOR PERFORMANCE

Creator performance was evaluated using the latest observed snapshot of each video.

This prevents cumulative daily view counts from being incorrectly added together.

Among creators with at least three videos, the highest aggregate engagement rates included:

* 5SOSVEVO — 22.26%
* ShawnMendesVEVO — 20.52%
* BANGTANTV — 19.62%
* Camila Cabello — 18.36%
* ibighit — 18.26%

The results demonstrate that creator scale and audience engagement are not interchangeable.

For example, ibighit combines substantial reach with strong engagement, while other creators achieve high engagement rates with a much smaller content footprint.

This distinction is important for a creator platform because creator performance should consider both audience scale and the quality of audience interaction.

### CONTENT PERFORMANCE

The highest-reach videos in the dataset include:

* Childish Gambino — This Is America: approximately 225.21M views
* YouTube Rewind: The Shape of 2017: approximately 149.38M
* Ariana Grande — No Tears Left To Cry: approximately 148.69M
* Becky G & Natti Natasha — Sin Pijama: approximately 139.33M
* BTS — FAKE LOVE: approximately 123.01M

The strongest-performing content by reach is concentrated heavily around music releases, major entertainment properties, and high-profile creators.

However, reach should be evaluated together with engagement rate because high views do not automatically indicate proportional audience interaction.

### CATEGORY PERFORMANCE

Music generated approximately **4.83 billion latest-snapshot views**, followed by Entertainment at approximately **2.83 billion**.

Other major categories by reach included:

* Film & Animation — approximately 814.52M views
* Comedy — approximately 773.84M
* People & Blogs — approximately 667.66M
* Sports — approximately 639.39M

Engagement performance tells a different story.

Among categories with at least 50 observed videos, Comedy recorded approximately **4.26% engagement**, followed closely by Howto & Style at approximately **4.23%**.

Education and People & Blogs also recorded engagement rates close to 4%.

This demonstrates why AICines should evaluate both content reach and audience response rather than relying solely on total views.

### TRENDING AND DISCOVERY ACTIVITY

The dataset contains **205 observed trending dates**.

Most days contain approximately 200 trending observations, with the observed daily range being 196–200.

This indicates that the trending dataset has a relatively stable daily observation capacity.

As a result, raw daily row volume should not be interpreted directly as audience demand.

A more useful discovery analysis is to monitor:

* Unique videos entering or appearing in trending
* Active creators
* Number of days individual videos remain trending
* Creator-level trending persistence

This provides a more meaningful view of the discovery ecosystem.

### CREATOR CONSISTENCY

The analysis measures how long individual videos remain in the trending dataset and then aggregates those results to the creator level.

The longest observed individual trending durations included:

* Sam Smith — 29 days
* Lucas and Marcus — 29 days
* grav3yardgirl — 28 days
* 20th Century Fox — 28 days
* Complex — 28 days

At creator level, zefrank1, Gibi ASMR, Bleacher Report, and VH1 were among creators with high average trending duration among those with at least three observed videos.

This metric provides a different perspective from total views.

A creator with repeated content that remains discoverable for longer periods may demonstrate stronger consistency within the platform's discovery ecosystem even if the creator does not have the highest total reach.

### AUDIENCE ENGAGEMENT

Three core engagement metrics were created:

**Like Rate**

Likes ÷ Views × 100

**Comment Rate**

Comments ÷ Views × 100

**Overall Engagement Rate**

(Likes + Comments) ÷ Views × 100

Across the latest video observations, the combined engagement rate was approximately **3.16%**.

These measures allow content to be compared relative to its audience size.

The analysis also identified individual videos with substantially higher engagement rates than the overall dataset, demonstrating that high audience interaction can occur even when total reach is comparatively smaller.

## RECOMMENDATIONS

Based on the analysis, AICines-style creator and content teams can use the following analytical priorities:

### 1. Evaluate creators using multiple performance dimensions

Creator performance should not rely exclusively on views.

A more complete creator-performance framework should combine:

* Reach
* Engagement rate
* Number of videos
* Trending persistence
* Average trending duration

This prevents high-volume creators from automatically being treated as the strongest performers.

### 2. Separate reach from engagement

High-reach content should be monitored alongside engagement rate.

This allows the platform to identify:

* High-reach/high-engagement content
* High-reach/low-engagement content
* Lower-reach/high-engagement content

These groups can support different creator-development and content-discovery strategies.

### 3. Monitor category-level performance

Category performance should be monitored using both total reach and engagement.

The results show that the category with the largest audience reach does not necessarily produce the highest engagement rate.

### 4. Track trending persistence

Trending duration provides a useful discovery metric beyond total views.

Monitoring how long videos remain discoverable can help identify creators and content formats that consistently maintain audience interest.

### 5. Build creator performance monitoring around repeatable KPIs

The SQL framework can be converted into recurring reporting metrics for:

* Creator reach
* Creator engagement
* Video performance
* Category performance
* Trending persistence
* Audience interaction

This creates a foundation for ongoing creator-performance monitoring rather than a one-time analysis.

## ASSUMPTIONS AND CAVEATS

1. **Daily snapshot structure:** The dataset records videos repeatedly across trending dates. Views, likes, and comments are cumulative snapshot values and should not be summed across every daily observation for video-performance analysis.

2. **Latest video snapshot:** Creator and content performance use the latest observed record for each `video_id`.

3. **Trending duration:** A video's trending duration is measured as the number of distinct dates on which it appears in the dataset. It should not be interpreted as a continuous uninterrupted streak unless the dates are explicitly checked for continuity.

4. **Duplicate observations:** The original uploaded CSV contained 48 exact duplicate rows and 50 duplicate `video_id + trending_date` combinations. Duplicate handling was therefore applied before the final analytical dataset was used.

5. **Category mapping:** The source dataset contains category IDs. Readable category names were mapped for analysis.

6. **Engagement rate:** Engagement is calculated as `(likes + comments) / views`. This metric does not include shares because the source dataset does not contain a share-count field.

7. **Disabled interactions:** Some videos have comments or ratings disabled. This can affect engagement comparisons, particularly when comment-based metrics are interpreted.

8. **Trending dataset limitation:** Appearing in the trending dataset is not equivalent to being representative of all YouTube content or all YouTube users.

9. **Historical scope:** The analysis represents the period contained in the dataset and should not automatically be treated as a current representation of YouTube behaviour.

10. **Data preparation:** Python was used for acquisition and preparation, PostgreSQL for the main analytical work, and Power BI for visualization.

## NEXT STEPS

1. Extend the analysis to additional regional datasets to compare creator and category behaviour across markets.

2. Develop creator-level segmentation based on reach, engagement, content volume, and trending persistence.

3. Add a more detailed discovery analysis measuring video entry, continued presence, and exit from the trending dataset.

4. Build a reusable PostgreSQL analytical layer or views so Power BI can connect directly to validated SQL outputs.

5. Expand the Power BI report with interactive creator, category, content, engagement, and trending-performance views.

6. Explore campaign and attribution data in a future AICines-focused project to connect creator performance with business outcomes.

## About

This project is an AICines-tailored YouTube content and creator performance analysis designed to demonstrate an end-to-end Data Analyst workflow.

The project uses the Kaggle API and Python for data acquisition and preparation, PostgreSQL and SQL for the primary analytical work, and Power BI for visualization.

The analysis focuses on creator performance, content reach, category performance, trending and discovery behaviour, creator consistency, and audience engagement.

The SQL work demonstrates practical analytical techniques including CTEs, window functions, ranking, aggregation, duplicate handling, date analysis, joins, calculated KPIs, and business-oriented metric design.
