# financial-analytics-powerbi-dashboard
End-to-end Power BI dashboard analyzing financial transactions, customer demographics, and risk — built with Power Query, DAX time intelligence, and a star schema data model.


---
## What's in it

- Overview page with key numbers - total amount, total transactions, average value, fees, tax - and how they compare year over year
- Transaction status tracking (successful, pending, failed) and risk scores by customer segment
- A toggle to switch the charts between Amount, Fees, Tax, and Transactions
- Drill-through pages so you can click into a month or into failed transactions and see the actual records behind them

## Tools used

- Power BI Desktop
- Power Query for cleaning the data
- DAX for the calculations and time intelligence stuff

## Data

- The data is in Excel format. I used a star schema - one fact table and two dimension tables:

- **Finance_Transactions** (fact table) - around 50k rows, has transaction id, amount, fees, tax, status, channel, category, risk score
- **Customers** (dimension) - about 5k customers with age, gender, state, city, segment
- **Calendar** - a date table I built for the time intelligence functions like SAMEPERIODLASTYEAR

## Cleaning the data

Before building anything I went through Power Query and:
- checked for missing/null values
- removed duplicate rows based on customer id and transaction id
- trimmed extra spaces from text columns like channel type
- made currency values consistent (uppercase INR)
- combined first name and last name into one customer name column

## Some of the DAX measures

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

## Insights found

- Total transaction value for 2025 was ₹137.53M across 14.94K transactions, up 1.41% from the previous year even though the number of transactions actually dropped slightly (-0.57%). Average transaction value went up 1.98%, so people are transacting less often but for bigger amounts.
- Most of the money (85.37% / ₹117.4M) is tied to successful transactions, but ₹14.4M (10.44%) is sitting in failed transactions and ₹5.8M (4.19%) in pending - that's about 14.6% of total transaction value not going through cleanly, which is worth digging into.
- Retail customers dominate - they account for ₹76M of the total amount, more than all other segments combined (Premium ₹26M, SME ₹21M, Corporate ₹9M, Wealth ₹6M).
- Maharashtra leads by state (₹22M), followed by Karnataka (₹16M) and Gujarat (₹15M). The top 3 states alone make up close to 40% of the total amount.
- Deposits are the biggest transaction type by value (₹20.3M) and also generate the most in fees (₹34.5K) and tax (₹6.2K). Card Payments and Investments follow behind.
- Split between genders is fairly close - Female customers account for 52.77% (₹72.6M) of total amount vs Male at 47.23% (₹65.0M).
- Looking at the month-wise trend, transaction amounts dipped noticeably in February and September, and peaked around July - could be seasonal, worth checking against real calendar events if this were a live business dataset.

## How to open this

1. Clone or download this repo
2. The data files are in the Data folder (Excel files)
3. Open the .pbix file in Power BI Desktop
4. If it asks you to update the data source path, just point it to the Data folder and click Close & Apply

## Files

```
├── Data/                       # excel files used
├── Financial_Analysis.pbix     # the power bi file
└── README.md
