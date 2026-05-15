<h1 align="center">✈️ British Airways — Data Science Job Simulation</h1>

<p align="center">
  <strong>Professional Virtual Work Experience · The Forage</strong><br>
  End-to-end data science across demand forecasting and customer behaviour prediction
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-The%20Forage-0A66C2?style=flat-square" alt="Forage">
  <img src="https://img.shields.io/badge/Company-British%20Airways-002147?style=flat-square" alt="British Airways">
  <img src="https://img.shields.io/badge/Field-Data%20Science-E01937?style=flat-square" alt="Data Science">
  <img src="https://img.shields.io/badge/Language-Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Status-Completed-28a745?style=flat-square" alt="Completed">
</p>

---

## Table of Contents

- [About This Simulation](#about-this-simulation)
- [Simulation Summary](#simulation-summary)
- [Task 1 — Lounge Demand Modelling](#task-1--lounge-demand-modelling)
- [Task 2 — Customer Booking Prediction](#task-2--customer-booking-prediction)
- [Key Results at a Glance](#key-results-at-a-glance)
- [Skills Developed](#skills-developed)
- [Repository Structure](#repository-structure)
- [Business Problems Solved](#business-problems-solved)
- [Analytical Thinking Developed](#analytical-thinking-developed)
- [How My Thinking Changed](#how-my-thinking-changed)
- [What I Would Do Differently](#what-i-would-do-differently)
- [Key Learnings & Reflections](#key-learnings--reflections)
- [Connect](#connect)

---

## About This Simulation

This repository documents my completion of the **British Airways Data Science Job Simulation** on [The Forage](https://www.theforage.com/simulations/british-airways/data-science-yqoz) — a professional virtual work experience programme designed by British Airways to simulate real work on their Data Science team.

Across two tasks, I worked on genuine business problems faced by BA's **Airport Planning** and **Customer Acquisition** teams — building a demand forecasting lookup table for Heathrow Terminal 3 lounges, and training a machine learning model to predict customer booking behaviour.

> This is not a tutorial project. Every dataset, business problem, and deliverable mirrors real work done inside British Airways.

---

## Simulation Summary

| Detail | Info |
|---|---|
| **Company** | British Airways |
| **Programme** | Data Science Job Simulation |
| **Platform** | The Forage |
| **Duration** | ~4 hours (self-paced) |
| **Completed** | May 2026 |
| **Location** | Bengaluru, Karnataka, India |
| **Certificate** | [View Certificate](certificates/) |
| **Simulation Link** | [The Forage — BA Data Science](https://www.theforage.com/simulations/british-airways/data-science-yqoz) |

> ⚠️ Per Forage's referencing policy, this is listed as a **virtual experience / extracurricular activity** — not work or internship experience. [View Policy](https://cdn-assets.theforage.com/Forage+Referencing+Policy+V5.pdf)

---

## Task 1 — Lounge Demand Modelling

### Business Context
BA's Airport Planning team needed a scalable way to forecast lounge demand at **Heathrow Terminal 3** for future flying schedules — without relying on specific flight numbers or aircraft details that may change.

### What I Built
A **reusable 8-category lounge eligibility lookup table** that maps flight groupings to Tier 1 / Tier 2 / Tier 3 lounge eligibility percentages — applicable to any future schedule.

### Dataset
| Property | Value |
|---|---|
| Rows | 10,000 flights |
| Columns | 17 |
| Source | BA Summer Schedule (Heathrow T3) |
| Nulls | 0 |

### Approach

```
Raw Schedule (10,000 flights)
        ↓
Grouped by: HAUL (SHORT/LONG) × TIME_OF_DAY (Morning/Lunchtime/Afternoon/Evening)
        ↓
Calculated avg eligibility % per group using historical TIER_ELIGIBLE_PAX data
        ↓
Output: 8-row reusable lookup table + written justification
```

### Final Lookup Table

| Flight Group | Tier 1 % | Tier 2 % | Tier 3 % | Flights |
|---|---|---|---|---|
| Long-haul · Morning | 0.22% | 2.95% | 11.25% | 1,461 |
| Long-haul · Lunchtime | 0.19% | 2.89% | 11.06% | 435 |
| Long-haul · Afternoon | 0.21% | 2.94% | 11.17% | 939 |
| Long-haul · Evening | 0.23% | 2.86% | 10.97% | 1,190 |
| Short-haul · Morning | 0.34% | 4.36% | 16.74% | 2,069 |
| Short-haul · Lunchtime | 0.39% | 4.55% | 17.28% | 757 |
| Short-haul · Afternoon | 0.34% | 4.31% | 16.59% | 1,366 |
| Short-haul · Evening | 0.33% | 4.45% | 17.00% | 1,783 |

> **Note:** Tier 1 (Concorde Room) is modelled as hypothetical at Terminal 3 — the near-zero % across all groups supports this as a future planning assumption, not a confirmed facility.

### Key Insight
Short-haul flights show **~50% higher Tier 3 eligibility** than long-haul routes — not because they have more premium passengers, but because the smaller A320 aircraft (fewer total seats) concentrates the percentage. This was a non-obvious finding that emerged directly from the data.

### Deliverable
`Lounge Lookup table/` → Completed Excel template with lookup table + 4-question written justification

---

## Task 2 — Customer Booking Prediction

### Business Context
BA's commercial strategy requires proactive customer acquisition — identifying customers **before** they reach the airport. The goal was to build a model that predicts whether a customer will complete a booking, enabling targeted marketing interventions.

### Dataset
| Property | Value |
|---|---|
| Raw rows | 50,000 |
| After deduplication | 49,281 |
| Columns | 14 |
| Target variable | `booking_complete` (0/1) |
| Class split | 85% not booked · 15% booked |

### Pipeline

```
Load CSV (50,000 rows)
    ↓ Remove 719 duplicates
    ↓ Verify zero nulls
    ↓ Engineer: total_add_ons = baggage + seat + meals
    ↓ Label encode 5 categorical columns
    ↓ Handle 85/15 imbalance → class_weight='balanced'
    ↓ Stratified 80/20 train/test split
    ↓ Train Random Forest (100 trees, max_depth=10)
    ↓ Evaluate: ROC-AUC, precision/recall, StratifiedKFold CV
    ↓ Output: feature importance chart + business insights
```

### Model Configuration

| Parameter | Value | Reason |
|---|---|---|
| Algorithm | Random Forest Classifier | Interpretable feature importance |
| n_estimators | 100 | Stable ensemble voting |
| max_depth | 10 | Prevents overfitting |
| class_weight | balanced | Handles 85/15 class imbalance |
| Cross-validation | StratifiedKFold (5-fold) | Preserves class ratio per fold |

### Model Results

| Metric | Value |
|---|---|
| **ROC-AUC (test set)** | **0.7628** |
| **CV ROC-AUC (mean)** | **0.7616** |
| CV Std Deviation | ±0.0018 |
| Recall — Booked class | 69% |
| Precision — Booked class | 30% |
| Overall Accuracy | 71% |

> CV scores across 5 folds: `[0.7608, 0.7631, 0.7637, 0.7621, 0.7585]` — near-zero variance confirms the model generalises reliably and is not overfitting.

### Feature Importance

| Rank | Feature | Importance | Business Meaning |
|---|---|---|---|
| 1 | `booking_origin` | 36.8% | Where customer books from — strongest cultural buying signal |
| 2 | `route` | 14.2% | Specific routes attract more committed travellers |
| 3 | `length_of_stay` | 10.8% | Longer planned trips = higher commitment |
| 4 | `flight_duration` | 9.8% | Longer flights tied to more deliberate planning |
| 5 | `purchase_lead` | 7.7% | Days before departure signals urgency/intent |
| 6 | `flight_hour` | 5.0% | Time of flight correlates with traveller type |
| 14 | `trip_type` | 0.5% | Nearly no predictive power |

### Business Insights From Analysis

| Finding | Insight |
|---|---|
| Malaysia (34.5%), Macau (31.6%), Vietnam (29.5%) | 2× above-average conversion — priority marketing markets |
| 0–7 days before departure = 17.8% booking rate | Last-minute customers convert highest — trigger urgency campaigns |
| Short stays (1–7 days) = 19.3% conversion | Weekend-getaway segment is most likely to complete bookings |
| Afternoon flights (13:00–18:00) = up to 19.5% | Peak booking hours — optimal window for retargeting ads |
| 3 add-ons selected = 18.6% vs 0 add-ons = 10.7% | Multi-add-on customers are a high-intent micro-segment |

### Deliverable
`Booking Analysis/` → Jupyter notebook with full pipeline · `BA_Booking_Prediction_Model.pptx` → 6-slide executive summary

---

## Key Results at a Glance

```
┌─────────────────────────────────────────────────────────┐
│  TASK 1 — LOUNGE DEMAND MODELLING                       │
│  ✔ 8-category lookup table from 10,000-flight dataset   │
│  ✔ Data-driven Tier eligibility % (not manually guessed)│
│  ✔ Applicable to any future BA flight schedule          │
│  ✔ Tier 1 ~0.29% → confirmed hypothetical at T3        │
├─────────────────────────────────────────────────────────┤
│  TASK 2 — BOOKING PREDICTION MODEL                      │
│  ✔ ROC-AUC: 0.763 (test) · 0.762 ±0.002 (CV)          │
│  ✔ 69% recall on minority (booked) class                │
│  ✔ Identified top 3 markets: Malaysia, Macau, Vietnam   │
│  ✔ 14 features ranked by predictive importance          │
│  ✔ Model stable across all 5 cross-validation folds     │
└─────────────────────────────────────────────────────────┘
```

---

## Skills Developed

### Technical Skills

| Skill | Applied In |
|---|---|
| Python (pandas, NumPy) | Data cleaning, feature engineering, groupby aggregation |
| Scikit-learn | Random Forest, train/test split, cross-validation, metrics |
| Matplotlib | Feature importance chart, 6 business insight charts |
| OpenPyXL | Writing lookup table programmatically to Excel |
| Label Encoding | Converting 5 categorical columns for ML compatibility |
| Class Imbalance Handling | `class_weight='balanced'`, StratifiedKFold |
| Data Visualisation | ROC curve, confusion matrix, business bar/line charts |
| PowerPoint (pptxgenjs) | 6-slide BA-branded executive presentation |

### Professional & Analytical Skills

- Translating raw operational data into **reusable business planning tools**
- Designing **scalable models** that work on unseen future schedules
- Framing ML output as **business recommendations**, not just metrics
- Identifying and handling **class imbalance** without data leakage
- Communicating technical findings to a **non-technical audience**
- Building executive-level **data-driven presentations**

---

## Repository Structure

```
BRITISH-AIRWAYS-Data-Science-Job-Simulation/
│
├── Lounge Lookup table/
│   └── task1_analysis.ipynb                   ← Python EDA + groupby code
│
├── Booking Analysis/
│   └── task2_model.ipynb                      ← Full ML pipeline
│
├── BA_Booking_Prediction_Model.pptx           ← 6-slide executive summary
└── README.md                                  ← This file
```

---

## Business Problems Solved

**1. Scalable Lounge Capacity Planning**
BA's Airport Planning team couldn't forecast lounge demand on schedules that didn't exist yet. I built a lookup table that decouples planning from specific flight details — any future schedule can be mapped to demand estimates using just two variables (HAUL + TIME_OF_DAY).

**2. Proactive Customer Acquisition**
Rather than waiting for customers to book, the model scores browsing customers by booking probability — enabling BA to intervene with targeted messaging before abandonment. The model identifies which regional markets, routes, and timing signals drive the highest conversion.

**3. Class Imbalance in Real-World Data**
Real booking data is never balanced — only 15% of sessions result in a booking. A naive model achieves 85% accuracy by predicting "no booking" every time. I solved this with `class_weight='balanced'` and validated stability using StratifiedKFold, producing a model that genuinely captures bookers rather than ignoring them.

---

## Analytical Thinking Developed

- **Grouping strategy matters more than the model itself** — choosing HAUL × TIME_OF_DAY over other combinations directly determines whether the lookup table is useful in practice
- **ROC-AUC is more honest than accuracy** for imbalanced classification — accuracy can be misleading when one class dominates
- **Cross-validation variance tells you as much as the mean** — a ±0.002 std deviation confirmed the model's reliability far better than a single test score
- **Feature importance is a business tool, not just a model output** — booking_origin at 36.8% directly shapes where BA should allocate marketing budget

---

## How My Thinking Changed

**Before:** I expected long-haul, premium-route flights to dominate lounge eligibility — intuition based on First Class associations.

**After:** Short-haul flights showed consistently higher Tier 3 eligibility (~17% vs ~11%) because frequent European business commuters with Executive Club membership fly on smaller aircraft — a pattern invisible without actually running the numbers.

**Before:** I expected purchase_lead time to be the #1 predictor of booking completion — more planning time = stronger intent.

**After:** booking_origin dominated at 36.8% importance. Where a customer is booking from captures cultural behaviour, purchasing power, and intent signals that no single numerical feature can match.

---

## What I Would Do Differently

- **Task 1:** Add ARRIVAL_REGION as a third grouping variable to create a 24-row lookup table — would capture Asia vs Europe demand differences that HAUL alone doesn't distinguish
- **Task 2:** Explore SMOTE (Synthetic Minority Oversampling) alongside class_weight to see if recall on the booked class could be pushed above 75%
- **Task 2:** Test XGBoost alongside Random Forest for a direct AUC comparison — tree ensembles often outperform Random Forest on tabular imbalanced data
- **Both tasks:** Build an automated pipeline that takes a new flight schedule CSV as input and outputs a demand forecast and booking probability scores in one step

---

## Key Learnings & Reflections

> "The most valuable insight from Task 1 wasn't the table itself — it was learning that a well-designed 8-row reference beats a 10,000-row calculation every time when the goal is scalability."

> "Task 2 taught me that ML model quality isn't the hardest problem. The hardest problem is asking the right business question first — 'how do we catch the bookers we're missing' rather than 'how do we maximise accuracy'."

- Real airline data is messier than academic datasets — 719 duplicates in 50,000 rows is typical in production data pipelines
- Business stakeholders care about **what to do**, not what the ROC-AUC score is — every chart in the final deck was framed as an action, not a metric
- Data science at an airline sits at the intersection of operations, marketing, and strategy — you need all three lenses simultaneously

---

## Connect

<p>
  <a href="https://www.linkedin.com/in/manjunathareddyn/">
    <img src="https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin" alt="LinkedIn">
  </a>
  &nbsp;
  <a href="https://github.com/MrBeastGamerZz">
    <img src="https://img.shields.io/badge/GitHub-MrBeastGamerZz-181717?style=flat-square&logo=github" alt="GitHub">
  </a>
</p>

---

<p align="center">
  <sub>British Airways Data Science Job Simulation · The Forage · April 2026</sub><br>
  <sub>This simulation was completed independently as part of a structured job-readiness programme.</sub>
</p>

