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
Summitt Energy is a Canadian energy retailer offering electricity, natural gas, and home services. Their Operations team supports customer enrollment, billing, and retention across multiple provincial utilities. To mirror their real‑world data challenges, this project:

- **Generated** a synthetic dataset of **6,000** customers (2021–2024) with:
  - Messy text fields (regions, plan names, utility providers, sales channels)
  - Realistic pricing, usage, contract lengths, and support ticket counts
- **Aligned** each customer’s utility provider to their province, simulating multi‑system data integration
- **Built** a **three‑page** interactive Power BI dashboard to track enrollment trends, churn behavior, and revenue performance

---

## Executive Summary  
- **Enrollment:** 6,000 total customers, 948 cancellations (churn rate 15.8%)  
- **Churn:** Mid‑contract (7–12 months) is the highest cancellation window; Natural Gas customers show the highest churn (16.9%)  
- **Revenue:** Flat‑rate subscription revenue of \$472 K/month undercaptures potential \$1.12 M/month usage‑based billing by nearly 2.8×  
- **Plan & Provider Performance:** Flex plans exhibit the lowest churn (14%); Enbridge and Hydro Quebec customers churn at rates above 19%  
- **Regional Variations:** Nova Scotia and Manitoba have the longest average tenure (~36 months); Saskatchewan and Quebec show shorter tenures (~32–33 months)

---

## Dashboard Overview  
The Power BI solution consists of three bookmarked pages, accessed via left‑pane buttons:

1. **Enrollment**  
   - Tracks customer counts, churn split, tenure distribution, monthly enrollment trends, and geographic spread  
2. **Churn**  
   - Analyzes churn rates by product type, contract length bucket, plan tier, utility provider, and provincial geography  
3. **Revenue**  
   - Compares subscription vs. usage‑based revenue by sales channel, product type, provider, and province; includes monthly revenue trend  

A **date‑range slider** atop every page allows dynamic filtering between 2021 and 2024.

---

## Objective  
Deliver an end‑to‑end Operations Analytics tool that enables Summitt Energy to:

- **Monitor** customer acquisition and attrition in a single pane  
- **Identify** high‑risk segments by product, plan, provider, and region  
- **Compare** flat subscription revenue with actual usage‑based billing  
- **Pinpoint** optimal timing and tactics for retention campaigns  
- **Support** data‑driven decisions across Sales, Marketing, Customer Service, and Compliance teams  

---

## Key Metrics Analyzed  
- **Enrollment & Tenure**  
  - Total versus churned customer counts  
  - Monthly enrollment trends  
  - Average tenure by province and tenure bucket  
- **Churn & Retention**  
  - Overall churn rate  
  - Churn rate by product type, plan name, contract length, utility provider  
  - Cancellation timing within contract lifecycle  
- **Revenue & Billing**  
  - Flat‑rate (subscription) revenue from active customers  
  - Estimated usage‑based billing (Usage × PricePerUnit)  
  - Revenue breakdown by sales channel, product type, provider, province  
  - Month‑over‑month revenue trends  
- **Plan & Provider Analysis**  
  - Plan tier performance (e.g., Flex vs. Green)  
  - Provider‑specific churn and revenue contributions  

---

## Technologies Used  
- **Power BI Desktop**: Data ingestion, modeling, and interactive visualization  
- **Power Query (M language)**: Advanced data cleaning and transformation tasks  
- **DAX (Data Analysis Expressions)**: Creation of calculated columns and measures  
- **Python & Faker library**: Synthetic dataset generation and initial profiling  

---

## Dataset Description  
| Field                     | Description                                                                                                                                       |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| `CustomerID`              | Unique customer identifier (`CUST00001`–`CUST06000`)                                                                                               |
| `EnrollmentDate`          | Date of service enrollment (2021–2024)                                                                                                             |
| `CancelDate`              | Date of service cancellation (if any), always ≥ `EnrollmentDate`                                                                                    |
| `Region`                  | Messy province string (e.g., “b.c.”, “ONTARIO ”) for cleaning practice                                                                             |
| `Province`, `City`        | Clean Canadian province and city                                                                                                                   |
| `ProductType`             | Service category: Electricity, Natural Gas, or Home Services                                                                                        |
| `PlanName`                | Subscription tier (Basic, Standard, Premium, Flex, Green) with typos/variants for cleanup                                                          |
| `ContractDurationMonths`  | Contract term in months (6, 12, 18, 24)                                                                                                            |
| `Price`                   | Flat monthly subscription fee (CAD)                                                                                                                |
| `Usage`                   | Monthly consumption (kWh for Electricity, GJ for Natural Gas, null for Home Services)                                                               |
| `UsageUnit`               | Unit of `Usage` (“kWh”, “GJ”, or “N/A”)                                                                                                            |
| `PricePerUnit`            | Assumed utility rate: \$0.12/kWh or \$3.50/GJ                                                                                                      |
| `EstimatedMonthlyBill`    | Calculated as `Usage * PricePerUnit` (CAD)                                                                                                         |
| `SalesChannel`, `Source`  | Enrollment channel and marketing source (messy values)                                                                                             |
| `UtilityProvider`         | Messy provider names mapped to canonical utility per province (e.g., “Hydro-One” → “Hydro One”)                                                    |
| `SupportTickets`          | Number of customer support cases opened                                                                                                            |
| Derived Columns           | `IsCancelled`, `TenureMonths`, `ContractLengthBucket`, `TenureBucket`                                                                               |

---

## Analysis Process  

### Data Cleaning  
- **Trimmed**, **Cleaned**, and **Capitalized** text columns to remove whitespace, non‑printable characters, and standardize casing.  
- **Replaced Values** for known typos/variants:  
  - PlanName: “Prmium” → “Premium”, “Standrad” → “Standard”  
  - SalesChannel: “ phone” → “Phone”, blank → “Unknown”  
  - UtilityProvider: mapped “hydro one”, “Hydro-One” → “Hydro One”, etc.  
- **Validated Dates** to ensure `CancelDate` ≥ `EnrollmentDate`; regenerated where necessary.  
- **Mapped Utility Providers** to match each customer’s province for realistic alignment.

### Data Transformation  
- **Added** `IsCancelled` flag (Boolean) for churn segmentation.  
- **Created** `UsageUnit` and `PricePerUnit` to distinguish kWh vs. GJ billing logic.  
- **Calculated** `EstimatedMonthlyBill` to simulate usage‑based billing revenue.  
- **Binned** `ContractDurationMonths` into `ContractLengthBucket` (e.g., “0–6 mo”, “7–12 mo”).  
- **Computed** `TenureMonths` from enrollment to cancel/today, and bucketed into `TenureBucket` for churn timing.

### DAX Calculations  
- **Calculated Columns**:  
  - `TenureMonths` = `DATEDIFF(EnrollmentDate, CancelDate or TODAY(), MONTH)`  
  - `ContractLengthBucket` and `TenureBucket` using `SWITCH(TRUE(),…)` logic  
- **Measures (Examples)**:  
  - `Total Customers` = `COUNTROWS()`  
  - `Churn Rate` = `DIVIDE(CountCancellations, Total Customers)`  
  - `Avg Tenure (Months)` = `AVERAGE(TenureMonths)`  
  - `Total Subscription Revenue` = `SUMX(FILTER(IsCancelled=No), Price)`  
  - `Total Estimated Billing` = `SUM(EstimatedMonthlyBill)`  
  - `Churn Rate by Product` = `CALCULATE([Churn Rate], ALLEXCEPT(ProductType))`

### Visualization Design  
- **KPI Cards**: Top‑level metrics for quick health check  
- **Donut & Bar Charts**: Active vs. churn split; churn segmentation by tenure, product, plan, provider  
- **Area/Line Charts**: Enrollment and revenue trends over time  
- **Maps**: Geospatial distribution of customers, churn, and revenue by province  
- **Bookmarks & Buttons**: Seamless navigation between pages; synchronized year slicer

---

## Key Insights  
1. **Churn Timing**  
   - **7–12 months** sees the highest cancellations (~240), followed by end‑of‑contract (24+ months).  
2. **Product & Plan Performance**  
   - **Natural Gas** customers churn most (16.9%); **Flex** plans churn least (14%).  
3. **Regional Tenure**  
   - **Nova Scotia** and **Manitoba** show the longest average tenures (~36 months).  
4. **Revenue Gap**  
   - Usage‑based billing potential (\$1.12 M/mo) far exceeds flat subscription revenue (\$472 K/mo).  
5. **Provider Hotspots**  
   - **Enbridge** and **Hydro Quebec** customers churn at rates above 19%, indicating process or communication friction.

---

## Recommendations  
1. **Mid‑Contract Retention**  
   - Deploy automated outreach at 4–6 and 7–12 month milestones to reduce mid‑term churn.  
2. **Hybrid Pricing Models**  
   - Introduce plans with a base subscription plus variable usage component to capture additional revenue.  
3. **Provider Process Partnership**  
   - Collaborate with high‑churn utilities (Enbridge, Hydro Quebec) to streamline onboarding and billing notifications.  
4. **Plan Flexibility Roll‑Out**  
   - Expand “Flex” features into Standard and Premium tiers to emulate its lower churn profile.  
5. **Seasonal Campaigns**  
   - Mirror January enrollment success mid‑year (e.g., June/July promotions) to boost sign‑ups during slower periods.

---
*Authored by Anthony Ofordum • GitHub: [tonydum126](https://github.com/tonydum126) • April 2025*  
