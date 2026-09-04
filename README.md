# Financial Analysis Dashboard - Power BI

An interactive Power BI dashboard analyzing financial transaction data across 2023-2026, built to answer key business questions around transaction performance, customer segments, revenue margins, and operational risk.

## Features

- Overview page with key metrics - total amount, total transactions, average value, fees, tax - with year-over-year comparisons
- Year filter to view any single year (2023-2026) or compare across years
- Transaction status tracking (successful, pending, failed) and risk scoring by customer segment
- Dynamic metric toggle to switch charts between Amount, Fees, Tax, and Transactions
- Drill-through pages to view underlying transaction records for a given month or for failed transactions

## Tools used

- Power BI Desktop
- Power Query for data transformation
- DAX for calculations and time intelligence

## Data

Source data is in Excel format, spanning 4 years (2023-2026, with 2026 partial through April). Structured as a star schema with one fact table and two dimension tables:

- **Finance_Transactions** (fact table) - transaction id, amount, fees, tax, status, channel, category, risk score
- **Customers** (dimension) - age, gender, state, city, customer segment
- **Calendar** - a date table built to support time intelligence functions like SAMEPERIODLASTYEAR

## Data cleaning and transformation

Handled in Power Query:
- Checked for missing/null values across all columns
- Removed duplicate records based on customer id and transaction id
- Trimmed extra spaces from text columns like channel type
- Standardized currency values (uppercase INR)
- Combined first name and last name into a single customer name field

## Sample DAX measures

```DAX
Total Transactions =
DISTINCTCOUNT(Finance_Transactions[Transaction_ID])

Previous Year Transactions =
CALCULATE(
    [Total Transactions],
    SAMEPERIODLASTYEAR('Calendar'[Date])
)

YoY Transactions % =
VAR _YoY = [Total Transactions] - [Previous Year Transactions]
RETURN
DIVIDE(_YoY, [Previous Year Transactions], 0)
```

## Business questions this dashboard answers

**Is the business growing, shrinking, or stable year over year?**
Total transaction amount has stayed largely flat across the four years - ₹137.07M in 2023, ₹135.62M in 2024, ₹137.53M in 2025 - with transaction counts holding steady around 15K annually. 2026's partial-year figures (₹45.32M, 4.99K transactions till April) are in line with prior years. The business shows stability rather than growth or decline.

**What percentage of transactions are failing, and is that improving over time?**
The success/failed/pending proportions are consistent across every year - roughly 85% success, 10% failed, 4-5% pending from 2023 through 2026. A steady ~10% failure rate that isn't improving points to a systemic issue (e.g. a specific channel or transaction type) rather than an isolated event, and is the clearest area for operational follow-up.

**Which customer segments drive the most value, and has that mix shifted?**
Retail is the leading segment in every year, consistently around 54-55% of total amount (₹74M in 2023 and 2024, ₹76M in 2025), followed by Premium, SME, Corporate, and Wealth in the same order each year. Segment ranking and share have remained stable, with no meaningful shift in customer mix over time.

**Which transaction types generate the most revenue, and where do fees concentrate?**
Loan EMI is the top transaction type by value across all four years (₹38.7M in 2023, ₹39.9M in 2024, ₹40.0M in 2025), followed by Transfer and Investment. Fees and tax go up or down in line with the total transaction amount each year, staying at around 0.157-0.16% of the total. This means the fee structure is stable and predictable, and isn't being driven by any one transaction type

**Where is the customer base concentrated geographically?**
Maharashtra, Karnataka, and Gujarat are the top 3 states by transaction amount every year without exception, together accounting for a large share of total volume. This signals a geographically concentrated customer base that hasn't diversified over the four-year period - relevant for any expansion or regional marketing decisions.

**Is there a meaningful difference in transaction behavior across customer demographics?**
Gender distribution stays close to 50/50 across all years with no meaningful trend, suggesting transaction behavior at the aggregate level isn't strongly differentiated by gender. other segmentation (customer segment, state) is more useful for targeting.

## Summary

The overall structure of the business - the leading customer segment, top states, main transaction type, and total volume - has stayed consistent across all four years. This points to a stable, mature transaction base rather than one that is still changing. The one area that stands out for further review is the failed transaction rate, which has held steady at around 10% every year without improvement, indicating an ongoing operational issue that hasn't been addressed."

## How to run locally

1. Clone or download this repository
2. Data files are in the `Data/` folder (Excel format)
3. Open the `.pbix` file in Power BI Desktop

## Project structure

```
├── Data/                       # Excel source files
├── Financial_Analysis_Dashboard.pbix     # Power BI project file
├── Financial_Analysis_Dashboard.pdf  # Pdf version of the dashboard 
└── README.md
```
