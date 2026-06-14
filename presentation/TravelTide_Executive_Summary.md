# TravelTide Analytics — Executive Summary

**Project:** Customer Behavioral Segmentation & Rewards Program Perk Assignment  
**Analyst:** Sonali Biswas — MSIT Program  
**Date:** June 2026  
**Status:** Analysis Complete — Recommendations Ready for Implementation

---

## 1. Business Context

TravelTide is a travel booking platform experiencing a post-launch decline in Daily Active Users (DAU) following a peak in Q1 2023. The business needed to shift from reactive reporting to proactive, data-driven retention — specifically by understanding *how* users behave on the platform and assigning each user to a personalised rewards perk that would encourage re-engagement and increase booking frequency.

The core constraint: **every user must receive exactly one perk** so that marketing communications are clear, personalised, and actionable.

---

## 2. Objective

Design and implement a customer segmentation model using behavioural data (sessions, bookings, clicks, discounts, cancellations) to:

1. Identify meaningful user segments based on RFM scoring
2. Assign each user in the target cohort to one of five rewards perks
3. Validate perk effectiveness through a controlled A/B test
4. Provide strategic recommendations for the next perk programme iteration

---

## 3. Data Pipeline Summary

| Stage | Description | Record Count |
|---|---|---|
| Raw sessions | Full platform session log | ~Full DB |
| Post-Jan 4 2023 filter | Sessions after the target cohort start date | Reduced set |
| Users with >7 sessions | Minimum engagement threshold applied | ~5,998 users |
| `sessions_filtered` table | Final session-level working dataset | **~49,000 rows** |
| `user_agg_features` table | One row per user with aggregated behavioural features | **~5,998 users** |
| `user_perk_ab_testing` table | Users with assigned perk and A/B variant flag | **~5,998 users** |

### Cohort Definition (Critical Filter)

```sql
-- Two-stage filter to produce the ~49,000 session records:
-- Stage 1: Sessions after Jan 4, 2023
-- Stage 2: Only users who had MORE THAN 7 such sessions
WHERE session_start > '2023-01-04'
HAVING COUNT(*) > 7
```

---

## 4. Key Findings

### 4.1 Engagement & Conversion
- Users with **25+ page clicks** per session convert at **over 65%** — the checkout funnel works when users are deeply engaged
- **75% of flight-booking sessions** have no hotel attached — the single largest cross-sell revenue gap on the platform
- Peak engagement window: **17:00–21:00** — all campaigns should be timed to this window

### 4.2 User Demographics
- **67% of users** have no children — the platform skews toward independent travellers and couples
- **85–90% of US users are female** — a strength that also signals an untapped male audience
- Top home airports: **LGA, JFK, LAX** — hyper-local geo-targeting to NYC and LA will maximise ROAS
- Platform is underleveraged with both **retirees (60+)** and **Gen Z travellers (18–24)**

### 4.3 January 2023 Cohort
- Approximately **3,500 users** signed up in January 2023, driven by a Winter Sale Campaign
- This cohort is the most strategically valuable group — known acquisition channel, defined tenure, measurable behaviour
- Daily sign-ups peaked from ~20/day to **180/day** at campaign peak (Jan 10, 2023)

### 4.4 RFM Segmentation Results

| Segment | Users | Description |
|---|---|---|
| **Engaged** | ~5,439 | High monetary or moderate frequency — core audience |
| **Frequent** | ~9 | Very high session frequency |
| **Other** | ~550 | Low engagement, lower monetary value |

### 4.5 Perk Assignment Distribution

| Perk | Assignment Rule | Est. Users |
|---|---|---|
| Flight Discount | ≥2 flights booked AND avg flight discount >$50 | ~18% |
| Hotel Discount | ≥2 hotels booked AND avg hotel discount >$50 | ~15% |
| Family Perk | Has children AND avg nights booked ≥3 | ~12% |
| Loyalty Perk | ≥10 sessions created | ~27% |
| New User Perk | Sign-up date ≥ Jan 2026 | ~8% |
| Other | No qualifying condition met | ~20% |

### 4.6 A/B Test Results
- **Result: Neutral** — Control and Treatment retention rates were statistically equivalent across all four perk types
- **Interpretation:** The segmentation methodology is sound; the *incentive design* is the variable to optimise
- The experimentation measurement window was based on historical sign-up dates (3–5 years back), which dilutes the perk signal significantly

---

## 5. Strategic Recommendations

### Recommendation 1 — Target Low-Ceiling Segments
Prioritise the Family segment and budget-conscious travellers for the next perk iteration. These groups have the lowest baseline retention and therefore the most measurable headroom for uplift.

### Recommendation 2 — Redesign Perk Value Proposition
Current perks (points, tier access) did not compel behavioural change. Test higher-impact incentives:
- Instant cashback on the next booking
- Free hotel night after 3 flight bookings
- Priority boarding on the next 3 flights
Validate through 10–15 qualitative user interviews *before* scaling to another A/B test.

### Recommendation 3 — Fix the Experimentation Window
Restructure the next A/B test to measure from **Day 0 of perk delivery**, not from the user's historical sign-up date. This produces a clean, isolated signal that directly attributes retention change to the perk.

---

## 6. Deliverables

| Deliverable | Format | Location |
|---|---|---|
| Databricks Notebook | `.ipynb` | `notebooks/Traveltide_MPDS_7_1.ipynb` |
| Stakeholder Presentation | `.pptx` | `presentation/TravelTide_Analysis.pptx` |
| Executive Summary | `.md` | `docs/TravelTide_Executive_Summary.md` |
| User Perk Assignments | `.csv` | `data/traveltide_user_perk_assignments.csv` |
| README | `.md` | `README.md` |

---

*Prepared by Sonali Biswas · TravelTide Analytics Project · MSIT Program · June 2026*
