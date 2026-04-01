# Bank Loan Analytics Dashboard

## Project Overview

This Power BI dashboard provides a comprehensive analysis of a bank's lending portfolio, analyzing a dataset of **65,000+ loan records**. The project aims to monitor lending activities, assess credit risk, and improve decision-making regarding loan applications.

The dashboard helps stakeholders distinguish between "Good Loans" (Fully Paid) and "Bad Loans" (Defaults/Charged Off) while tracking key financial metrics like Funded Amount, Total Received Payments, and Interest Rates.

The project demonstrates skills in:
- **Data Cleaning & Processing** (Handling 65k raw records)
- **DAX** (Complex risk ratios and financial calculations)
- **Data Visualization** (Interactive KPI cards, Donut charts, and Maps)
- **UI/UX Design** (Custom Navigation & Drill-through actions)

## Dashboard Preview

### Home Page (Summary)
The Executive View provides a high-level summary of the bank's performance, splitting the portfolio into "Good Loans" vs. "Bad Loans" to immediately highlight risk exposure.
![Dashboard Home Screenshot](https://github.com/AnveshDhamane/Bank-Loan-Data-Analysis-Power-Bi/blob/1c3cc437b4d7b9ecd67bff31a11b4ae896c4988b/Main%20Dashboard.png)

---

## Navigation & Analysis Views

The dashboard features a navigation menu with distinct buttons, allowing users to switch between high-level summaries and granular geographical or operational reports.

### 1. Overview Dashboard (Home)
**Focus:** Strategic Performance & Risk Assessment.
**Description:** The central hub of the report. It aggregates total loan applications and financials. It splits the data into "Good Loan" vs. "Bad Loan" categories to track the health of the lending portfolio.
* **Key Visuals:**
    * *KPI Cards:* Total Loan Applications, Funded Amount, Amount Received, Avg Interest Rate.
    * *Loan Status Grid:* Breakdown of 'Fully Paid', 'Current', and 'Charged Off' loans.

### 2. Branch Location
**Focus:** Geographical Analysis.
**Description:** This view visualizes lending metrics across different States and Cities. It helps identify which regions have the highest borrowing demand and which areas pose higher default risks.
* **Key Visuals:**
    * *State Map:* Heatmap showing funded amounts by location.
    * *Regional Breakdown:* Tables displaying loan performance by State Abbreviation (`State_Abbr`) and Branch.

![Branch Location Screenshot](https://github.com/AnveshDhamane/Bank-Loan-Data-Analysis-Power-Bi/blob/97e14cfab9c8ae0103dc85dc1832233ab3834b2a/Branch%20Location.png)

### 3. Bank Report
**Focus:** Operational Reporting.
**Description:** A tabular and detailed view designed for auditors and branch managers. It provides granular data at the loan-ID level for compliance and auditing purposes.

![Bank Report Screenshot](https://github.com/AnveshDhamane/Bank-Loan-Data-Analysis-Power-Bi/blob/97e14cfab9c8ae0103dc85dc1832233ab3834b2a/Bank%20Report.png)

### 4. Drill-Through: Detailed Age Analysis
**Focus:** Demographic Risk Profiling.
**Description:** This advanced feature allows users to inspect specific age groups in depth.
* **Trigger:** Select any bar in the **"Sum of Funded Amount by Age"** chart on the Home page.
* **Action:** Click the **"Get Details"** button that appears.
* **Result:** Redirects to a detailed report page showing individual borrower lists, specific default rates, and credit profiles strictly for that selected age demographic.

![Drill Through Screenshot](https://github.com/AnveshDhamane/Bank-Loan-Data-Analysis-Power-Bi/blob/97e14cfab9c8ae0103dc85dc1832233ab3834b2a/Age%20Drill%20Through.png)

---

## Dataset Description

The dataset (`65k.xlsx`) consists of 65,000+ rows of banking data.

| Column Category | Key Attributes |
|:---|:---|
| **Borrower Details** | `Age`, `Employment Length`, `Home Ownership`, `Annual Income` |
| **Loan Details** | `Loan Amount`, `Funded Amount`, `Term`, `Int_Rate`, `Grade`, `Purpose` |
| **Status & Payments** | `Loan Status`, `Total Payment`, `Total Principal Received`, `Recoveries` |
| **Location** | `State`, `City`, `Zip Code` |

---

## Features

### 1. Key KPIs
- **Total Funded Amount:** The principal amount disbursed to borrowers.
- **Total Amount Received:** The actual money (Principal + Interest) recovered by the bank.
- **Average Interest Rate:** The cost of borrowing, indicating the portfolio's profitability.
- **Average DTI (Debt-to-Income):** A measure of borrowers' financial health.

### 2. Interactive Filters
- **Grade & Sub-Grade Slicers:** Allows risk managers to filter loans by credit rating (A-G).
- **Time Intelligence:** Filter data by Year, Month, or Quarter to spot seasonal trends.
- **Drill-Through:** Navigation from "Summary" to "Detailed Age Analysis".

---

## DAX Measures & Calculations

This project uses advanced DAX to create dynamic measures for risk analysis. Below is the documentation for the core calculations used.

### 1. Risk Analysis Measures

**Default Rate**
* **Code:**
    ```dax
    Default Rate = DIVIDE(
        COUNTROWS(FILTER('Banking Data', 'Banking Data'[Is Default Loan] = "Y")), 
        COUNTROWS('Banking Data')
    )
    ```
* **What it tells:** The percentage of the total loan portfolio that has defaulted.

**Delinquent Loan Rate**
* **Code:**
    ```dax
    Delinquent Loan Rate = DIVIDE(
        COUNTROWS(FILTER('Banking Data', 'Banking Data'[Is Delinquent Loan] = "Y")), 
        COUNTROWS('Banking Data')
    )
    ```
* **What it tells:** The percentage of active loans that are currently behind on payments.

### 2. Financial Performance Measures

**Funding Gap % (Difference In %)**
* **Code:**
    ```dax
    Difference In % = CALCULATE(
        (SUM('Banking Data'[Loan Amount]) - SUM('Banking Data'[Funded Amount])) / 
        SUM('Banking Data'[Loan Amount])
    ) * 100
    ```
* **What it tells:** The variance between the amount a customer requested and the amount the bank actually funded.

**Principal Repayment Ratio (Fake Recovery)**
* **Code:**
    ```dax
    Fake Recovery = DIVIDE(
        SUM('Banking Data'[Total Rec Prncp]), 
        SUM('Banking Data'[Total Pymnt])
    )
    ```
* **What it tells:** The proportion of the total payment received that goes towards paying off the principal balance (vs. interest/fees).

**Total Return Ratio (Recovery_Rate)**
* **Code:**
    ```dax
    Recovery_Rate = DIVIDE(
        SUM('Banking Data'[Total Pymnt]), 
        SUM('Banking Data'[Total Rec Prncp])
    )
    ```
* **What it tells:** This metric tracks the total revenue generated per unit of principal repaid.

---

## Key Insights

### 1. Loan Volume by Purpose
A significant portion of loans is taken for **Debt Consolidation**, indicating that many borrowers are using bank loans to manage existing high-interest debts.

### 2. Home Ownership Impact
Borrowers with **"Mortgage"** or **"Own"** home ownership status generally show lower default rates compared to those listed as **"Rent"**, suggesting asset ownership correlates with creditworthiness.

### 3. Term Length Sensitivity
Loans with **60-month terms** tend to have higher interest rates and a significantly higher default probability compared to **36-month term** loans.

---

## Strategic Recommendations

Based on the dashboard insights and data analysis, the following actions are recommended to optimize the bank's portfolio:

### 1. Restrict 60-Month Loan Terms
* **Observation:** Loans with **60-month terms** have a default rate of **2.04%**, which is significantly higher than the **1.34%** default rate for **36-month** loans.
* **Recommendation:** Tighten eligibility criteria for long-term loans or increase the interest rate premium to cover the added risk.

### 2. Regional Risk Management (Chhattisgarh & Bihar)
* **Observation:** The state of **Chhattisgarh (CG)** shows an abnormally high default rate of **4.37%**, followed by **Bihar (BR)** at **2.8%**.
* **Recommendation:** Conduct a localized audit of branch lending practices in these regions. Consider pausing aggressive expansion in Chhattisgarh until repayment behaviors stabilize.

### 3. Expansion Opportunities (Himachal & Uttarakhand)
* **Observation:** **Himachal Pradesh (HP)** and **Uttarakhand (UK)** show near-zero default rates, indicating a highly reliable borrower base.
* **Recommendation:** Increase marketing spend and loan offers in these "Safe Haven" states to maximize secure revenue.

### 4. Product Portfolio Review
* **Observation:** **"Solar"** and **"Bike"** loans have higher delinquency rates compared to "Business" or "Personal" loans.
* **Recommendation:** Re-evaluate the underwriting process for these specific loan products.

---
