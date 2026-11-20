# MARKETING-CAMPAIGN-INCREMENTALITY-ANALYSIS---PYTHON-SQL-DUCKDB-PANDAS-SCIPY-POWER-BI-

# Marketing Campaign Incrementality Analysis

**Evaluating the Incremental Impact of Digital Advertising Campaigns Using Experimental Data**

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-DuckDB-orange?logo=duckdb&logoColor=white)
![Analytics](https://img.shields.io/badge/Analytics-Experimentation-green)
![Visualization](https://img.shields.io/badge/Visualization-Power%20BI-yellow?logo=powerbi&logoColor=black)

---

## 📋 Table of Contents
- [1. Project Overview](#1-project-overview)
- [2. Business Problem](#2-business-problem)
- [3. Dataset](#3-dataset)
- [4. Tools and Technologies](#4-tools-and-technologies)
- [5. Analytical Methodology](#5-analytical-methodology)
- [6. Key Findings](#6-key-findings)
- [7. Business Insights and Recommendations](#7-business-insights-and-recommendations)
- [8. Limitations and Considerations](#8-limitations-and-considerations)
- [9. Project Deliverables](#9-project-deliverables)
- [10. Repository Structure](#10-repository-structure)
- [11. Conclusion](#11-conclusion)

---

## 1. Project Overview

This project evaluates the effectiveness of a digital advertising campaign by measuring its incremental impact on customer visits and conversions. Using the **Criteo Uplift Modeling Dataset**, the analysis compares customers assigned to a treatment group against a control group.

> **Core Objective:** Did the advertising campaign generate additional customer conversions, or would those conversions have happened anyway?

Rather than relying on total conversion counts alone, this project applies an incrementality-focused framework encompassing data quality checks, exploratory data analysis, conversion and visit lift calculations, statistical hypothesis testing, customer segmentation, advertising exposure analysis, and interactive Power BI reporting.

---

## 2. Business Problem

Marketing campaigns often generate thousands of conversions, yet not every conversion is caused by advertising—many customers would have converted organically without exposure. Evaluating campaigns solely on total conversions risks overestimating ad effectiveness and misallocating marketing budgets.

**Key Questions Addressed:**
* Did the treatment group achieve a higher conversion rate than the control group?
* How much incremental conversion and visit lift was observed?
* Are the differences between treatment and control statistically significant?
* Do campaign outcomes vary across customer segments?
* How does actual advertising exposure differ across segments?
* What actionable insights can guide campaign targeting and budget optimization?

---

## 3. Dataset

* **Dataset:** Criteo Uplift Modeling Dataset
* **Source:** [Kaggle – Uplift Modeling Dataset](https://www.kaggle.com/datasets/arashnic/uplift-modeling)
* **Scale:** ~13.98 million customer records across 16 columns

### Dataset Schema

| Column | Description |
| :--- | :--- |
| `f0` – `f11` | Anonymized numerical customer features |
| `treatment` | Treatment assignment (`1` = treatment, `0` = control) |
| `conversion` | Conversion outcome (`1` = converted, `0` = not converted) |
| `visit` | Visit outcome (`1` = visited, `0` = did not visit) |
| `exposure` | Actual advertising exposure indicator (`1` = exposed, `0` = not exposed) |

*Note: Because customer features are anonymized without real-world labels, they are treated strictly as numerical variables rather than assumed demographic or behavioral attributes.*

---

## 4. Tools and Technologies

| Tool | Purpose |
| :--- | :--- |
| **Python** | Data analysis and statistical calculations |
| **Pandas** | Data manipulation and preprocessing |
| **DuckDB** | High-performance SQL querying on large-scale data |
| **SciPy** | Statistical hypothesis testing and confidence intervals |
| **Google Colab** | Cloud development environment |
| **Power BI** | Interactive executive dashboard and reporting |
| **GitHub** | Version control and project documentation |

---

## 5. Analytical Methodology

### Step 1: Data Inspection and Quality Checks
* Examined dataset dimensions, column structures, and data types.
* Checked for missing values across key experimental and outcome variables.
* Validated binary encoding for `treatment`, `conversion`, `visit`, and `exposure`.
* Verified logical consistency (e.g., zero exposure in the control group, no conversions without visits).
* **Result:** Zero missing values were found in the four core binary variables, and all logical consistency checks passed.

### Step 2: Exploratory Data Analysis (EDA)
Analyzed the distributions of treatment assignment, customer features, visits, conversions, and actual ad exposure. Because group sizes differ substantially, rate-based metrics were prioritized over raw counts:
* **Control Group:** 2,096,937 customers
* **Treatment Group:** 11,882,655 customers

### Step 3: Campaign Incrementality Measurement
Using an **Intention-to-Treat (ITT)** approach (comparing assigned groups regardless of actual ad exposure), the following metrics were calculated:
* **Absolute Lift:** $\text{Rate}_{\text{Treatment}} - \text{Rate}_{\text{Control}}$
* **Relative Lift:** $\left(\frac{\text{Absolute Lift}}{\text{Rate}_{\text{Control}}}\right) \times 100$
* **Estimated Incremental Outcomes:** $\text{Absolute Lift} \times \text{Treatment Group Size}$

### Step 4: Statistical Significance Testing
* Conducted **two-proportion z-tests** to evaluate differences in conversion and visit rates.
* Computed a **95% confidence interval** for the overall conversion-rate difference.
* Applied a **Bonferroni-adjusted significance threshold ($\alpha = 0.01$)** across the five customer segment comparisons to control for multiple testing.

### Step 5: Customer Segmentation
Segmented customers into five groups based on the overall distribution of anonymized feature `f0`, comparing conversion rates, visit rates, incremental lifts, and exposure rates across each segment.

### Step 6: Advertising Exposure Analysis
Evaluated actual ad exposure within the treatment group and across the five segments. Because exposure is not randomized, these comparisons are treated as descriptive rather than causal.

### Step 7: Power BI Dashboard
Built an interactive dashboard featuring:
* Control vs. Treatment Conversion & Visit Rates
* Incremental Conversion Lift by Customer Segment
* Exposure Rate vs. Conversion Lift
* Executive KPI Cards (Overall Conversion Lift, Relative Lift, Visit Lift, and Treatment Exposure Rate)

---

## 6. Key Findings

### Overall Campaign Performance

| Metric | Control | Treatment | Absolute Lift | Relative Lift |
| :--- | ---: | ---: | ---: | ---: |
| **Customers** | 2,096,937 | 11,882,655 | — | — |
| **Conversions** | 4,063 | 36,711 | — | — |
| **Conversion Rate** | 0.1938% | 0.3089% | **+0.1152 pp** | **+59.45%** |
| **Visits** | 80,105 | 576,824 | — | — |
| **Visit Rate** | 3.8201% | 4.8543% | **+1.0342 pp** | **+27.07%** |

* **Statistical Validation:** Z-score = `28.52`, p-value = `7.308 × 10⁻¹⁷⁹`, 95% CI = `[+0.1085 pp, +0.1219 pp]`.
* **Estimated Incremental Impact:** Approximately **13,687 incremental conversions** and **122,895 incremental visits** across the treatment group.

### Customer Segment Performance (`f0` Segments)

| Segment | Control Conv. Rate | Treatment Conv. Rate | Incremental Conv. Lift | Treatment Exposure Rate |
| :---: | ---: | ---: | ---: | ---: |
| **Segment 1** | 0.1488% | 0.1870% | +0.0381 pp | 0.68% |
| **Segment 2** | **0.8245%** | **1.3686%** | **+0.5441 pp** | **19.11%** |
| **Segment 3** | 0.1580% | 0.2386% | +0.0806 pp | 2.80% |
| **Segment 4** | 0.0610% | 0.0777% | +0.0167 pp | 0.79% |
| **Segment 5** | 0.0363% | 0.0450% | +0.0087 pp *(n.s.)* | 0.45% |

* **Segment 2 Dominance:** Recorded the highest baseline conversion rate, the strongest incremental conversion lift (**+0.5441 pp**), and the largest incremental visit lift (**+5.0644 pp**).
* **Significance Across Segments:** Segments 1–4 achieved statistically significant conversion lift at $\alpha = 0.01$; Segment 5 did not meet the adjusted threshold. All five segments achieved statistically significant visit lift.
* **Exposure Disparity:** Overall treatment exposure was **3.60%** (428,212 of 11,882,655 customers), heavily concentrated in Segment 2 (**19.11%**).

---

## 7. Business Insights and Recommendations

1. **Prioritize High-Response Segments:** Focus deeper behavioral and media-delivery analysis on **Segment 2**, which drove the majority of incremental conversions and visits.
2. **Audit Campaign Delivery & Pacing:** Investigate why ad exposure was heavily skewed toward Segment 2 (19.11% vs. <3% in other segments) to ensure balanced delivery or intentional bid optimization.
3. **Adopt Incrementality as the Core KPI:** Shift reporting away from raw or last-click attributed conversions toward randomized treatment-vs.-control lift metrics.
4. **Maintain Experimental Rigor:** Preserve clean holdout control groups and monitor assignment balance and exposure logging in future campaigns.
5. **Integrate Financial Metrics:** Incorporate order revenue, customer lifetime value (LTV), and ad spend data into future experiments to calculate incremental **ROAS** and **ROI**.

---

## 8. Limitations and Considerations

* **Group Size Imbalance:** Treatment and control sample sizes are unequal (~85% vs. ~15%), requiring rate-based comparisons.
* **Unverified Randomization:** The upstream treatment assignment mechanism could not be independently audited.
* **Anonymized Features:** Features `f0`–`f11` lack business definitions, limiting qualitative persona interpretation.
* **Non-Random Exposure:** Actual ad exposure is subject to selection bias (e.g., user activity levels or bidding algorithms) and cannot be interpreted causally.
* **Missing Cost/Revenue Data:** Absence of financial fields prevents direct profitability, ROI, or ROAS calculation.

---

## 9. Project Deliverables

* Data inspection and quality-check pipeline
* Large-scale SQL analysis using DuckDB
* Statistical hypothesis testing and segmentation in Python
* Interactive Power BI executive dashboard
* Comprehensive project report (`.docx`)

---

## 10. Repository Structure

```text
marketing-campaign-incrementality/
│
├── data/
│   └── README.md                                       # Instructions to download the dataset
│
├── notebooks/
│   └── marketing_incrementality_analysis.ipynb         # End-to-end Python, DuckDB & SciPy analysis
│
├── dashboard/
│   └── marketing_incrementality_dashboard.pbix         # Interactive Power BI report
│
├── reports/
│   └── Marketing_Campaign_Incrementality_Project_Report.docx
│
├── images/
│   └── dashboard_screenshot.png                        # Preview of Power BI dashboard
│
└── README.md
