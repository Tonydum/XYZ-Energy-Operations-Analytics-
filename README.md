# XYZ Energy Operations Analytics Dashboard

## Table of Contents
1. [Project Background](#project-background)  
2. [Executive Summary](#executive-summary)  
3. [Dashboard Overview](#dashboard-overview)  
4. [Objective](#objective)  
5. [Key Metrics Analyzed](#key-metrics-analyzed)  
6. [Technologies Used](#technologies-used)  
7. [Dataset Description](#dataset-description)  
8. [Analysis Process](#analysis-process)  
   - [Data Cleaning](#data-cleaning)  
   - [Data Transformation](#data-transformation)  
   - [DAX Calculations](#dax-calculations)  
   - [Visualization Design](#visualization-design)  
9. [Key Insights](#key-insights)  
10. [Recommendations](#recommendations)  

---

## Project Background  
XYZ Energy’s Operations team needed a unified analytics solution to monitor customer enrollments, churn behavior, and subscription revenue across Canadian markets. To replicate their real‑world environment, we generated a synthetic dataset of **6,000** customers (2021–2024) with intentionally messy fields drawn from multiple systems. We then cleaned, transformed, and modeled the data to build a **three‑page Power BI dashboard**—covering Enrollment, Churn & Retention, and Revenue.

---

## Executive Summary  
- **6,000** total customers; **948** cancellations (15.8% churn).  
- **Mid‑contract churn hotspot**: most cancellations occur between **7–12 months**.  
- **Flat subscription revenue** of **\$472 K/month** from active customers.  
- **Natural Gas** customers exhibit the highest churn rate (16.9%).  
- **Flex** plans demonstrate the lowest churn (14%), indicating strong value perception.

---

## Dashboard Overview  
Left‑pane navigation buttons link to three bookmarked pages:

1. **Enrollment**  
2. **Churn & Retention**  
3. **Revenue**

A synchronized **date‑range slicer** (2021–2024) filters all visuals.

---

## Objective  
Provide XYZ Energy’s leadership with a single pane of glass to:  
- Track customer acquisition and retention in real time  
- Identify high‑risk segments by product, plan, provider, and region  
- Monitor flat‑rate subscription revenue performance  
- Highlight optimal timing for retention interventions

---

## Key Metrics Analyzed  
- **Enrollment Metrics:**  
  - Total vs. churned customers  
  - Monthly enrollment trends  
  - Average tenure by province  
  - Customer distribution by city and province  
- **Churn Metrics:**  
  - Overall churn rate  
  - Churn by product type, plan tier, contract length, utility provider  
  - Cancellation timing within contract lifecycle  
- **Revenue Metrics:**  
  - Total subscription revenue (flat fee)  
  - Revenue by sales channel, product type, provider, and province  
  - Month‑over‑month revenue trend

---

## Technologies Used  
- **Power BI Desktop** — data modeling and interactive reporting  
- **Power Query (M)** — ETL for cleansing and shaping data  
- **DAX** — calculated columns and measures  
- **Python & Faker** — synthetic dataset generation

---

## Dataset Description  
| Column                     | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| `CustomerID`               | Unique identifier (CUST00001–CUST06000)                                     |
| `EnrollmentDate`           | Date of customer signup                                                     |
| `CancelDate`               | Date of cancellation (if any), always ≥ `EnrollmentDate`                    |
| `Region`                   | Messy province string (e.g., “b.c.”, “ONTARIO ”) for cleanup practice       |
| `Province`,`City`          | Clean Canadian province and city                                            |
| `ProductType`              | Service: Electricity, Natural Gas, Home Services                            |
| `PlanName`                 | Subscription tier with typos/variants (Basic, Standard, Premium, Flex, etc.)|
| `ContractDurationMonths`   | Contract term (6, 12, 18, 24 months)                                        |
| `Price`                    | Flat monthly subscription fee (CAD)                                         |
| `SalesChannel`,`Source`    | Enrollment channel and lead source (messy then standardized)                |
| `UtilityProvider`          | Provider name aligned to province (messy variants cleaned)                  |
| `SupportTickets`           | Number of support cases opened                                              |
| Derived columns            | `IsCancelled`, `TenureMonths`, `ContractLengthBucket`, `TenureBucket`       |

---

## Analysis Process

### Data Cleaning  
- **Trimmed & standardized** text fields to remove extra spaces and non‑printable characters.  
- **Replaced** known typos/variants in `PlanName`, `SalesChannel`, `Source`, `UtilityProvider`.  
- **Ensured** `CancelDate` always follows `EnrollmentDate`; regenerated invalid dates.

### Data Transformation  
- **Mapped** utility providers to provinces, including messy variants for cleaning practice.  
- **Generated** `IsCancelled` flag for churn segmentation.  
- **Binned** `ContractDurationMonths` into `ContractLengthBucket` (e.g., “0–6 mo”, “7–12 mo”).  
- **Calculated** `TenureMonths` and bucketed into `TenureBucket` for churn‑timing analysis.

### DAX Calculations  
- **Calculated Columns:**  
  - `TenureMonths` = `DATEDIFF(EnrollmentDate, CancelDate or TODAY(), MONTH)`  
  - `ContractLengthBucket` & `TenureBucket` via `SWITCH(TRUE(),…)`  
- **Key Measures:**  
  - `Total Customers` = `COUNTROWS()`  
  - `Active Customers` & `Cancelled Customers` via `FILTER(IsCancelled)`  
  - `Churn Rate` = `DIVIDE(Cancelled, Total)`  
  - `Average Tenure (Months)` = `AVERAGE(TenureMonths)`  
  - `Total Subscription Revenue` = `SUMX(FILTER(IsCancelled=No), Price)`  
  - Revenue by segment = `SUM(Price)` with appropriate filters

### Visualization Design  
- **KPI Cards**: at the top of each page for instant metrics.  
- **Donut & Bar Charts**: Active vs. churn, churn segmentation by tenure, product, plan, provider.  
- **Line Charts**: Enrollment and revenue trends over time.  
- **Maps**: Geospatial distribution by province & city.  
- **Bookmarks & Buttons**: For seamless page navigation; global date‑range slicer for consistency.

---

## Key Insights  
1. **Mid‑Contract Churn**: Highest cancellations in **7–12 months** bucket, suggesting targeted outreach at month 6.  
2. **Product Vulnerability**: **Natural Gas** customers churn at **16.9%**—consider seasonally timed retention campaigns.  
3. **Plan Flexibility**: **Flex** plans lowest churn at **14%**—flexible options improve stickiness.  
4. **Provider Friction**: **Enbridge** and **Hydro Quebec** churn rates >19%—optimize handoff and communication processes.  
5. **Revenue Concentration**: Subscription revenue of \$472 K/mo concentrated in Ontario and Email/In‑Person channels.

---

## Recommendations  
1. **Retention Milestones**: Implement automated check‑ins/offers around month 4–6 and pre‑renewal at month 24.  
2. **Hybrid Plan Launch**: Pilot subscriptions with a small usage component to capture incremental revenue.  
3. **Flex Features Roll‑Out**: Introduce flexible billing terms to Standard and Premium tiers.  
4. **Provider Partnership**: Collaborate with high‑churn utilities to streamline enrollment workflows.  
5. **Seasonal Promotions**: Mirror January marketing tactics in mid‑year to drive enrollment during slower periods.

---
*Prepared by Anthony Ofordum • GitHub: [tonydum126](https://github.com/tonydum126) • April 2025*  
