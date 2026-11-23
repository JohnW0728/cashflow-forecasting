# Cash Flow Forecasting and Anomaly Detection

Team project for the **UMDAC Datathon 2025** (December 2025). Team of [n]: [teammate names].
[Result: Top 7 finalist. Add a link to the official announcement if you have one.]

The challenge company provided a large, messy set of real transaction records. The goal was to clean the data, spot unusual transactions, forecast future cash flow and present the findings in a dashboard.

> **The dataset is confidential and is not included in this repository.** The company is not named, and the notebook has its outputs cleared, so it shows the method but cannot be re-run without the data.

## My contribution

I handled the overall data cleaning and the machine learning part:

- Cleaned and prepared the raw transaction data.
- Built the anomaly flags and the cash-flow categories.
- Built and compared forecasting models and chose the best one for each time horizon.
- Prepared dashboard-ready datasets for the team's Power BI dashboard (not included here).

*(Edit this section to match exactly what you did. Teammates worked on the dashboard and presentation: [names].)*

## Data

About 84,500 transaction records across several sheets: the main transactions, a cash balance sheet, a category linkage table, a country mapping and exchange rates. The data was messy, with empty columns, system duplicates and mixed data types.

## Approach

### 1. Data cleaning and flagging
- Dropped empty and unused columns.
- Flagged **system duplicates** using a strict rule (same name, date, amount, counterparty, reference and assignment) and excluded them from analysis.
- Flagged **weekend transactions** but kept them, because they are part of the normal pattern.
- Flagged **statistical outliers** using a Z-score within each transaction category. They are marked for review, not deleted.
- Mapped each transaction to a **cash-flow bucket** (such as operating or financing) using the category linkage table.

### 2. Exploratory analysis
- Looked at inflows and outflows, the largest counterparties, and trends over time.
- Used seasonal decomposition to separate trend, seasonality and noise.
- Measured volatility using the coefficient of variation. Weekly net cash flow turned out to be highly volatile, which is why simple linear models were unlikely to work well.

### 3. Forecasting
Daily data is very noisy, so I aggregated it to **weekly net cash flow**.

- **Model comparison:** Random Forest (with lag features) versus Holt-Winters, tested on the last 4 weeks of known history.
- **Result:** Random Forest had clearly lower error (about 29% lower MAE than Holt-Winters on that window).
- **Final setup:**
  - **1-month forecast (operational view):** Random Forest.
  - **6-month forecast (strategic view):** Holt-Winters, chosen for stability and seasonality.

### 4. Dashboard data
Generated five optimised CSV files (forecast vs actual, driver breakdown, regional view, weekday patterns and anomalies) for the team's Power BI dashboard.

## Limitations

- The validation window is only 4 weeks, so the model comparison is a rough guide, not a robust benchmark.
- Cash flow is highly volatile and the history is short, so forecasts carry a lot of uncertainty.
- Only two models were compared in the final evaluation.
- Outliers were flagged for review, not confirmed as errors.

## Tools

Python, pandas, NumPy, scikit-learn, statsmodels, matplotlib, seaborn, Plotly. Dashboard: Power BI (built by the team).

## Repository contents

```
.
├── README.md
├── umdac_cashflow_forecasting.ipynb   (outputs cleared, no data)
└── .gitignore                         (excludes data files)
```
