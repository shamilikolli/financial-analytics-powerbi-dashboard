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

## What I found

- Could see which customer segments (retail, wealth, corporate, premium) were driving most of the transaction volume
- Pulled out the failed transactions separately so they could be reviewed
- Compared fees collected across channels (net banking, mobile, ATM, branch) to see which ones were more profitable

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
