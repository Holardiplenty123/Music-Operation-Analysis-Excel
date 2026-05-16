# Music Operation Dashboard — Analytical Report on a Music Streaming Platform

**Tool:** Microsoft Excel (Power Query · Power Pivot · DAX)


**Dataset Period:** January 2021 – December 2024


**Markets Covered:** 10 Global Markets


**Category:** Business Intelligence · Subscription Analytics · Revenue Analysis

---

## Project Title

**Music Operation Dashboard** — An analytical report that captures a music streaming platform and drives insight.

---

## Overview

This project is a full end-to-end business intelligence solution built entirely in Microsoft Excel. It analyses four years of operational data from a mid-size music streaming platform, covering listening behaviour, subscription lifecycle events, user demographics, and revenue performance across 10 global markets.

The dashboard was designed with one goal: to give leadership a clear, honest, and actionable view of where the business is growing, where it is losing money, and where the biggest opportunities lie.

---

## The Problem

The business was sitting on a large volume of data spread across multiple tables, but without a structured analytical framework, key questions were going unanswered:

- Which markets and user segments are actually driving revenue?
- Why is churn so high and what can be done about it?
- Are paid users genuinely more engaged than free users?
- Which subscription events are growing the business and which are shrinking it?
- Is there suspicious activity distorting the platform's performance metrics?

Raw data alone could not answer these questions. A structured analytical approach was needed — one that connected listening behaviour to subscription outcomes and revenue impact.

---

## My Approach

### Step 1 — Data Preparation in Power Query
The project started with three raw CSV files:
- **dim_user** — 961 users with demographics, tier, country, and fraud flags
- **fact_listening_session** — 224,078 session records with listen duration, skip behaviour, device, and revenue
- **fact_subscription_event** — 3,640 subscription events capturing every upgrade, downgrade, churn, signup, and retention event

Each file was loaded separately into Power Query where I performed:
- **Column splitting** — separating `listen_start_ts` into individual date and time columns
- **Country name mapping** — converting 2-letter ISO codes to full country names across all 3 tables
- **Null handling** — filling blank `from_tier` values with `"new_subscriber"` to preserve the meaning of new customer events rather than discarding them
- **Data type corrections** — ensuring date, time, text, and numeric columns were correctly typed before loading to the data model

### Step 2 — Data Modelling in Power Pivot
All three tables were loaded into Power Pivot and connected using a **star schema** design:

```
fact_listening_session
        |
        | user_id
        |
     dim_user  (centre/dimension table)
        |
        | user_id
        |
fact_subscription_event
```

`dim_user` sits at the centre with `user_id` as the primary key. Both fact tables connect to it on the many side. This structure ensures that filtering by any user attribute, country, tier, fraud flag, age,  correctly propagates across all measures.

### Step 3 — DAX Measure Development
I built a comprehensive library of DAX measures covering every analytical dimension required:

**Revenue measures:**
- `Total Revenue` — sum of estimated session-level revenue
- `Total MRR` — sum of mrr_after_usd across subscription events
- `Churn Revenue Raw` — MRR change filtered to churn events only
- `Expansion MRR` — MRR change filtered to upgrades
- `Contraction MRR` — MRR change filtered to downgrades and churn combined
- `Net MRR Change` — total MRR movement across all event types
- `ARPU` — total MRR divided by distinct user count

**Engagement measures:**
- `Avg Listen Duration` — average listen_seconds across all sessions
- `Total Sessions` — count of all session records
- `Skip Rate` — sessions where skipped = TRUE divided by total sessions
- `New Subscribers` — distinct users whose from_tier = new_subscriber
- `Engagement Score` — average listen seconds multiplied by session count per user

**Trend indicators:**
- `Revenue Change %` — year-over-year revenue movement
- `Revenue Trend` — dynamic ▲/▼ indicator with formatted percentage
- `MRR Trend` — dynamic ▲/▼ indicator for MRR movement
- `Churn Rate` — churned distinct users divided by total users

### Step 4 — Dashboard Design
The dashboard was built across three interactive views accessible via navigation buttons at the top:

**View 1 — Dashboard (Overview)**
The primary landing view. Shows country revenue comparison, plan-level MRR distribution, subscription event distribution, upgrade source analysis, device performance, and monthly revenue trend. Designed for the business stakeholder who wants the full picture at a glance.

**View 2 — Analysis**
The analytical deep-dive view. Shows KPI cards with trend indicators (Total Revenue, Total MRR, Churn Rate, Total Sessions, Avg Listen Duration, Churn Revenue), plan revenue comparison, device session analysis, upgrade source breakdown, and monthly revenue trend. Designed for the analyst or manager who needs to interrogate the numbers.

**View 3 — Executive Summary**
The leadership view. Shows KPI cards, average listen duration by subscription tier, ARPU table by country, country revenue ranking, and monthly revenue trend. Stripped back to only what leadership needs to make decisions, no noise, no clutter.

**Interactive filters (slicers):**
- Year (2020–2024)
- Subscription tier
- Country

All charts and KPI cards respond dynamically to slicer selections, allowing stakeholders to drill into any combination of year, tier, and market.

---

## Tools Used

| Tool | Purpose |
|---|---|
| **Microsoft Excel** | Primary platform for the entire project |
| **Power Query** | Data cleaning, transformation, and loading |
| **Power Pivot** | Data modelling and star schema design |
| **DAX** | Measure creation for all KPIs and calculations |
| **Excel Charts** | Bar, column, line, donut, and waterfall visualisations |
| **Conditional Formatting** | KPI trend colouring and data quality highlighting |

---

## Key Insights Discovered

### 1. France Is the Highest Revenue Market — But Not the Biggest
France generated $121.04 in session revenue and leads all 10 markets despite having only 98 users, not the highest user count. Canada has 126 users but generates less revenue. This proves that **engagement quality, not user volume, drives revenue**, a critical finding for market investment decisions.

### 2. Free Plan Generates the Highest MRR — But at a Cost
The Free plan generates $12,858.96 in MRR, the highest of all three tiers. However, Free users also have the highest skip rate (20%) and the lowest average listen duration (78.4 seconds). The MRR figure is driven by volume of users, not quality of engagement. **Free users are the biggest revenue source and the biggest churn risk simultaneously.**

### 3. Upgrades Are the Growth Engine — But Churn Is Closing the Gap
Upgrades generated $11,106.31 in MRR growth. Churn and downgrades combined eroded $5,665.46. The net MRR expansion of $8,143.50 looks healthy on the surface, but with a 47.1% churn rate, the business is running a leaky bucket. **Every two users gained through upgrades, nearly one is lost through churn.**

### 4. Windows Users Drive the Most Sessions
Windows leads all devices with 52,707 sessions, nearly 50% more than the next platform (macOS with 44,320). iOS follows closely at 47,111. Android, despite being the most globally widespread mobile platform, records only 35,020 sessions, suggesting the Android user experience or app performance may be underperforming relative to other platforms.

### 5. Reconciliation Is the Largest Single MRR Event
At $2,648.24, reconciliation events account for the highest total MRR impact of any single upgrade source, exceeding organic ($1,493.74) and referral ($819.25). Reconciliation represents system corrections, not organic growth. **This is a data quality flag that changes how the revenue story is read**  true organic MRR growth is lower than headline figures suggest.

### 6. December Is the Peak Revenue Month — Likely Holiday Effect
The monthly revenue trend shows a significant December spike at $1,763.60, the highest single month across the 4-year period. This is consistent with holiday and festive activity driving increased listening and subscription activity. The pattern suggests **seasonal campaigns timed for November-December could amplify this natural peak.**

### 7. The Fraud Cluster Is a Critical Data Integrity Risk
50.2% of users — 482 out of 961, are flagged as part of a fraud cluster. This means all engagement metrics (sessions, listen duration, skip rate, revenue) must be treated as indicative rather than conclusive until fraud cluster users are excluded and reanalysed. **The true health of the platform's engagement may look significantly different once the fraud cluster is isolated.**

### 8. Family Tier Has the Highest Engagement Quality
Despite generating less MRR than Premium, Family tier users have the highest average listen duration (99 seconds) and the lowest skip rate (9.2%). This makes Family users the most engaged segment on the platform, a finding that has direct implications for content strategy and retention investment prioritisation.

---

## Challenges and How I Solved Them

**Challenge 1 — Blank from_tier values**
The `from_tier` column had blanks for all new subscribers. Filling with mode would have distorted the data. I created a custom Power Query column using `if [from_tier] = null then "new_subscriber" else [from_tier]` , preserving the real meaning of the blank rather than replacing it with noise.

**Challenge 2 — Skipped column stored as boolean, not number**
The `skipped` column stored TRUE/FALSE as boolean values, causing `SUM` to fail. I used `CALCULATE(COUNTROWS(...), [skipped]=TRUE())` in DAX to count boolean TRUE values correctly, avoiding the type mismatch that was breaking the formula.

**Challenge 3 — Many-to-many relationship risk**
Linking two fact tables directly creates a many-to-many relationship that Power Pivot cannot resolve correctly. I avoided this entirely by routing all relationships through `dim_user` as the central dimension table, following star schema best practice and ensuring all measures filtered correctly.

**Challenge 4 — Inflated percentage measures**
Early versions of Revenue Change % and Churn Rate were returning values like 2018% and 6608% due to dividing totals by small monthly denominators. I rebuilt both measures using year-over-year comparison for revenue and distinct user counts for churn, returning accurate, meaningful percentages.

---

## Actionable Insights

### What Was Discovered and Why It Matters

**Churn is the most urgent business risk.**
A 47.1% churn rate means nearly one in two users left the platform over the four-year period. Combined with $2,432.86 in lost MRR from churn events alone, this is not a data observation — it is a business emergency. Every revenue gain from upgrades is being partially cancelled by users walking out the door. Until churn is addressed, sustainable MRR growth is impossible.

**Free users are both the biggest revenue source and the biggest liability.**
The Free plan generates $12,858.96 in MRR — the highest of all tiers. But Free users skip 1 in 5 tracks and average only 78.4 seconds of listen time per session compared to 99 seconds for Family users. The platform is heavily dependent on a tier that is least committed to it. This imbalance creates fragile revenue that is one cancellation decision away from disappearing.

**France punches above its weight — and shows what good looks like.**
With 98 users and $121.04 in revenue, France generates more value per user than any other market. Canada has 126 users but earns less. France's engagement quality — not its size — is what drives its revenue leadership. This matters because it tells the business where to look when setting benchmarks for other markets.

**Reconciliation events are masking true organic growth.**
At $2,648.24, reconciliation is the single largest MRR driver in the upgrade source breakdown — exceeding organic ($1,493.74) and referral ($819.25). Reconciliation represents system corrections, not real customer-driven growth. Stripping it out reveals that organic revenue performance is significantly lower than headline figures suggest.

**The fraud cluster puts every metric in question.**
With 50.2% of users flagged as a fraud cluster, the reliability of all platform metrics — sessions, engagement, revenue — must be treated with caution. If fraud cluster users are artificially inflating session counts, the true average listen duration, skip rate, and engagement scores for genuine users could look very different from what the dashboard currently reports.

---

### Recommended Next Steps With Expected Impact

**1. Launch a churn prevention programme immediately.**
Identify users whose listen duration has declined by more than 30% over a 30-day rolling window and trigger an automated retention offer before they cancel. Target the 453 already-churned users with a re-engagement campaign offering a discounted return plan. Expected impact: recovering 10% of churned users restores approximately $243 in monthly MRR.

**2. Run a Free-to-Paid conversion campaign targeting heavy Free users.**
Identify the top 25% of Free users by session count and listen duration and offer them a 30-day Premium trial. France, Canada, and the United States are the priority markets given their session volume and revenue performance. Expected impact: a 10% conversion rate on 311 Free users adds approximately $311 in monthly recurring revenue.

**3. Conduct a full fraud cluster audit before the next reporting cycle.**
Separate fraud cluster users from the clean user base and rerun all key metrics — sessions, listen duration, skip rate, revenue, on clean users only. This produces a true picture of genuine platform health and eliminates the distortion that is currently baked into every headline number. Expected impact: restated metrics that leadership can trust and act on with confidence.

**4. Capitalise on the December revenue peak with a seasonal campaign.**
December is the highest revenue month at $1,763.60 — driven by holiday and festive activity. A structured November promotional campaign (limited time upgrade offer, gifting feature, family plan push) timed to capture the rising seasonal wave could meaningfully amplify this natural peak. Expected impact: even a 15% uplift on the December baseline adds approximately $264 in a single month.

**5. Invest in the Sweden market or reassess its priority.**
Sweden has the lowest user count (73), the fewest sessions (14,829), and the lowest revenue ($57.24) of all 10 markets. Either a targeted localisation and acquisition campaign is needed to bring it up to average market performance, or resources should be reallocated to higher-performing markets. Expected impact: bringing Sweden to the average market revenue level would add approximately $47 in monthly revenue.

---

### Areas for Deeper Investigation and Additional Data

**Content performance data is missing and it matters.**
The dataset contains artist, track, genre, and playlist IDs but no corresponding dimension tables. This means the analysis cannot answer which content is driving the most plays, revenue, or discovery events. Adding dimension tables for artists, tracks, and genres would unlock a full content performance layer, including which catalogue investments are paying off and which are not.

**Device dimension data would sharpen platform strategy.**
Windows leads all platforms with 52,707 sessions but without a device dimension table the analysis is limited to device IDs. A full device dimension would reveal device type, operating system version, and platform category, enabling proper mobile vs desktop vs smart TV comparison and informing where product investment should be prioritised.

**Pre-churn behaviour signals need longitudinal tracking.**
The current dataset shows what happened, churn events, but not the listening behaviour trajectory in the weeks leading up to each churn event. Building a pre-churn behaviour model using rolling averages of listen duration and skip rate in the 30 and 60 days before a churn event would create a leading indicator that gives the business time to intervene before the user leaves.

**Fraud cluster severity scoring is needed.**
The current fraud flag is binary, a user is either in the fraud cluster or not. A severity score based on session frequency, listen duration patterns, and revenue anomalies would allow the business to distinguish confirmed bot accounts from mildly suspicious ones, enabling a more targeted and proportionate response.

**Cohort analysis would reveal retention patterns over time.**
Grouping users by their signup month and tracking their subscription status month by month would show how long users typically stay, when churn risk peaks, and whether certain cohorts perform better than others. This is essential for understanding the true lifetime value of different user segments and optimising acquisition spend accordingly.

---


## Deliverables

- ✅ 3-view interactive Excel dashboard (Dashboard · Analysis · Executive Summary)
- ✅ Star schema data model in Power Pivot
- ✅ Full DAX measure
- ✅ Power Query transformation pipeline across 3 tables
- ✅ Full analytical Word report with executive summary, findings, and recommendations

---

*Built with Microsoft Excel · Power Query · Power Pivot · DAX*
