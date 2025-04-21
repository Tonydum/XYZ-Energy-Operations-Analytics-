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
XYZ Energy’s Operations team needed a unified analytics solution to monitor customer enrollments, churn behavior, and subscription revenue across Canadian markets. To replicate their real‑world environment, we generated a synthetic dataset of **6,000** customers (2021–2024) with intentionally messy fields drawn from multiple systems. We then cleaned, transformed, and modeled the data to build a **three‑page Power BI dashboard**—covering Enrollment, Churn & Retention, and Revenue.

---

## Executive Summary  
XYZ Energy’s Operations Analytics Dashboard delivers a comprehensive, data‑driven view of customer behavior, churn dynamics, and subscription revenue performance across Canada for the period **January 2021 through April 2025**. By synthesizing enrollment, tenure, churn, and revenue metrics into a single interactive report, we uncovered actionable insights to optimize retention, pricing, and operations.

**Key Findings:**  
- **Enrollment Trends:**  
  - **6,000** total customer accounts; after an initial marketing surge in **January 2021** (630 enrollments), monthly sign‑ups stabilized around **480–550**.  
  - Ontario (2,100), Quebec (1,200), and Alberta (900) account for **70%** of the customer base, with smaller provinces showing growth potential.  

- **Churn Dynamics:**  
  - Overall churn sits at **15.8%** (948 cancellations).  
  - **Mid‑contract churn** peaks between **7–12 months** (~240 cancellations), with a secondary rise at the **24‑month** renewal point (~190).  
  - **Natural Gas** customers churn highest (16.9%), followed by Electricity (15.3%) and Home Services (15.2%).  
  - **Provider hotspots**: Enbridge customers churn at **21%**, Hydro Quebec at **19.5%**, indicating friction during the utility handoff or billing process.  
  - **Plan performance**: “Flex” plans exhibit the lowest churn (~14%), demonstrating the value of flexible terms.  

- **Subscription Revenue:**  
  - Active customers generate **$472K/month** in flat subscription fees.  
  - **Channel breakdown**: In‑Person ($110K), Email ($108K), Phone ($105K), Online ($100K), with **$60K** still uncategorized—data‑capture improvements needed.  
  - **Product mix**: Electricity drives **$220K/mo**, Natural Gas **$160K**, and Home Services **$100K**.  
  - **Geographic revenue**: Ontario ($170K), Quebec ($90K), Alberta ($70K); under‑penetration in Western and Atlantic provinces.  

**Strategic Implications:**  
- **Retention Focus**: Mid‑contract (months 4–12) is the most critical window—deploy automated check‑ins, loyalty bonuses, or value‑added offers to reduce churn.  
- **Pricing Innovation**: Gap between flat‑rate revenue and actual usage opportunity suggests piloting hybrid plans (base fee + usage component) to capture incremental monthly billing.  
- **Plan Design**: Expand “Flex” features into higher‑tier plans to leverage its lower churn profile.  
- **Provider Collaboration**: Partner with Enbridge and Hydro Quebec to streamline enrollment handoffs, billing notifications, and customer communications.  
- **Marketing Cadence**: Replicate the successful January campaign mid‑year to boost enrollments during slower months.  

This dashboard transforms complex, multi‑source operational data into targeted, actionable intelligence—empowering XYZ Energy to enhance customer lifetime value, refine pricing strategy, and improve cross‑functional processes across Sales, Marketing, Customer Service, and Compliance teams.

---

## Dashboard Overview  
Left‑pane navigation buttons link to three bookmarked pages—Enrollment, Churn & Retention, and Revenue—each filtered by a global date‑range slicer (2021–2024).

![Dashboard](https://github.com/user-attachments/assets/373acbd4-e117-456e-a921-aa3263f3de0c)

### 1. Enrollment Page  
- KPI Cards: Total Customers, Churned Customers, Churn Rate, Avg Tenure, Total Subscription Revenue  
- Donut Chart: Active vs. Churned split  
- Bar Chart: Average Tenure by Province  
- Bar Chart: Cancellations by Tenure Bucket  
- Line Chart: Monthly Enrollments  
- Map: Customer counts by Province & City

![Dashboard2](https://github.com/user-attachments/assets/9e567fd6-9a4e-4ff0-9c98-a5dac886feee)

### 2. Churn & Retention Page  
- Bar Chart: Churn Rate by Product Type  
- Bar Chart: Cancellations by Contract Length Bucket  
- Bar Chart: Churn Rate by Plan Name  
- Bar Chart: Cancellations by Utility Provider  
- Map: Churned customers by Province

![Dashboard3](https://github.com/user-attachments/assets/438efb94-86df-427f-8a7a-438832c85124)

### 3. Revenue Page  
- Bar Chart: Subscription Revenue by Sales Channel  
- Bar Chart: Revenue by Product Type  
- Bar Chart: Revenue by Utility Provider  
- Line Chart: Monthly Revenue Trend  
- Map: Revenue by Province  
- KPI Cards: Flat subscription revenue metrics

---

## Objective  
Provide XYZ Energy’s leadership with an interactive reporting tool to monitor customer acquisition and retention, identify high‑risk segments, and track subscription revenue performance—all in real time.

---

## Key Metrics Analyzed  
- Enrollment counts and churn split  
- Average tenure by region  
- Monthly sign‑up trends  
- Overall and segment‑specific churn rates  
- Subscription revenue by channel, product, provider, and province

---

## Technologies Used  
- Power BI Desktop  
- Power Query (M)  
- DAX  
- Python + Faker (for data simulation)

---

## Dataset Description  
- 6,000 customer records  
- Includes: Customer ID, EnrollmentDate, CancelDate, ProductType, PlanName, Price, ContractDurationMonths, Province, City, SalesChannel, UtilityProvider, SupportTickets  
- Generated and transformed to include derived fields such as:  
  - IsCancelled  
  - TenureMonths  
  - ContractLengthBucket  
  - TenureBucket  

---

## Analysis Process

### Data Cleaning  
- Removed whitespace and standardized casing  
- Cleaned up inconsistent labels (e.g., “Hydro-One” → “Hydro One”)  
- Ensured logical date sequencing (CancelDate ≥ EnrollmentDate)  

### Data Transformation  
- Created buckets for tenure and contract durations  
- Aligned provider names with provincial standards  
- Added status flags for analysis (e.g., churned vs. active)

### DAX Calculations  
- TenureMonths  
- Churn Rate  
- Subscription Revenue  
- Average Tenure  
- Revenue by Dimension (Plan, Channel, Provider, Province)

### Visualization Design  
- Clean, bookmark‑driven navigation  
- Page‑specific KPI cards for fast interpretation  
- Segment‑level breakdowns and geo‑maps for storytelling  
- Slicers for dynamic filtering

---

## Key Insights  
1. Most customers churn between 7–12 months of tenure.  
2. Natural Gas customers churn at the highest rate (16.9%).  
3. Flex plans retain more customers than any other tier (~14% churn).  
4. Revenue is driven primarily by customers in Ontario and through In‑Person and Email channels.  
5. Enbridge and Hydro Quebec users show the highest churn and deserve focused operational review.

---

## Recommendations  
1. Deploy automated retention efforts at 4–6 and 11–12 months.  
2. Experiment with hybrid pricing models to improve revenue capture.  
3. Expand Flex plan offerings and embed flexibility into more tiers.  
4. Audit onboarding experience for high‑churn providers.  
5. Launch mid‑year marketing campaigns to replicate January’s success.

---

**Prepared by:** Anthony Ofordum  
**GitHub:** [github.com/tonydum126](https://github.com/tonydum126)  
**Date:** April 2025
