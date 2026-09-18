# Telco Customer Churn & Revenue Retention Audit

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15%2B-blue.svg)](https://www.postgresql.org/)
[![Power BI](https://img.shields.io/badge/Power_BI-Desktop-gold.svg)](https://powerbi.microsoft.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📌 Executive Summary
Customer churn presents a critical threat to subscription telecommunication businesses. This project delivers an end-to-end data audit of **7,043 subscriber accounts** from the IBM Telco dataset to diagnose primary churn drivers, quantify lost Monthly Recurring Revenue (MRR), and prescribe high-impact retention interventions.

The complete workflow covers:
1. **Data Ingestion & Integrity Auditing** via cloud PostgreSQL (`Aiven` / `pgAdmin 4` / `DBeaver`).
2. **Data Modeling & Feature Engineering** via a clean semantic view (`v_clean_customer`).
3. **Exploratory Data Analysis (EDA)** isolating multi-factor churn cohorts.
4. **Interactive BI Dashboard Architecture** built in Power BI Desktop.

---

## 📊 Dashboard Preview
![Telco Churn Dashboard](bi/dashboard_preview.png)

---

## 🔑 Key Performance Indicators (KPIs)
* **Total Customer Base:** 7,043
* **Total Churned Customers:** 1,869
* **Baseline Churn Rate:** 26.54%
* **Lost Monthly Recurring Revenue (MRR):** $139,130.85
* **Critical Risk Cohort Churn Rate:** **71.07%**

---

## 🔍 Analytical Insights & Findings

### 1. Contract Structure Vulnerability
* **Month-to-month contracts** exhibit a **42.71% churn rate**, accounting for 88.6% of all churned subscribers.
* In contrast, **One-year contracts** (11.27%) and **Two-year contracts** (2.83%) demonstrate steep churn resistance, confirming contractual lock-in as an effective churn barrier.

### 2. The First-Year Drop-Off Cliff
* Customers in their **first 12 months** churn at **47.44%**.
* Retention improves dramatically past the first year: **13–24 months (28.71%)**, **25–48 months (20.39%)**, and **49+ months (9.51%)**. 
* Onboarding and early customer success during months 1–6 represent the most critical intervention window.

### 3. The Fiber Optic & Tech Support Paradox
* **Fiber Optic** is the premium tier ($91.50/mo average bill) but suffers a **41.89% churn rate** compared to DSL (18.96%).
* Subscribers on **Fiber Optic without Tech Support** experience a staggering **49.37% churn rate**.
* Adding **Tech Support** cuts Fiber Optic churn down to **22.63%** (a 26.74 percentage point reduction), despite those customers paying higher average monthly charges ($101.18 vs. $87.74).

### 4. Billing Friction
* **Electronic check** users experience a **45.29% churn rate**, whereas automated payment methods (Credit Card and Bank Transfer) sit between 15% and 17%.

### 5. Multi-Dimensional "Red Zone" Cohort
Subscribers who meet all four vulnerability criteria:
* **Contract:** Month-to-month
* **Tenure:** $\le 12$ months
* **Service:** Fiber Optic
* **Support:** No Tech Support

**Result:** Represents only **11.8% of the total customer base** (833 accounts), yet produces **31.7% of all churned customers** (592 churned accounts) with an acute **71.07% churn rate**.

---

## 🎯 Strategic Business Recommendations

| Strategy | Action Item | Target Metric |
| :--- | :--- | :--- |
| **Support Bundling** | Automatically include 90-day complimentary Tech Support on all new Fiber Optic sign-ups. | Reduce early fiber churn from 49.4% to <25%. |
| **Contract Migration** | Offer a 10% monthly discount or complimentary streaming add-on for migrating from M-to-M to a 12-month commitment. | Transition accounts out of the 42.7% churn bucket. |
| **Payment Incentivization** | Provide a one-time $10 bill credit for switching from Electronic Check to automated credit card/bank transfer. | Mitigate payment-friction churn (45.3% to ~16%). |

---

## 🛠️ Repository File Structure

```text
telco-churn-retention-audit/
├── README.md
├── sql/
│   ├── 01_data_cleaning_and_views.sql
│   └── 02_cohort_risk_queries.sql
├── bi/
│   ├── Telco_Customer_Churn_Dashboard.pbix
│   └── dashboard_preview.png
└── data/
    └── dataset_source.md
```

---

## 💻 SQL Pipeline Architecture

### Data Quality Diagnosis
Identified 11 records where `tenure = 0` and `TotalCharges` was populated with whitespace (`' '`), failing numeric casting. Resolved via `NULLIF(TRIM(TotalCharges), '')::NUMERIC`.

View the full schema definition in [`sql/01_data_cleaning_and_views.sql`](sql/01_data_cleaning_and_views.sql).