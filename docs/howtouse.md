# 📘 Portfolio Analytics Lab — How to Use

Welcome to **Portfolio Analytics Lab**, a portfolio analytics dashboard designed to reconstruct your portfolio from transactions and provide deep insight into performance, attribution, and risk.

This guide explains:

- How to enter transactions manually  
- How to bulk upload transaction data  
- How Portfolio Lab interprets your data
- How to read performance, risk, and exposure diagnostics  
- Common mistakes to avoid  

---

## 🚀 Getting Started

1. Launch the app:
   [![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://portfolio-analytics-lab.streamlit.app)
Navigate to:
👉 Transactions (sidebar)

Each row represents one financial event in your portfolio. Next we will explain which inputs are required from you.

| Field | Description |
| :--- | :--- |
| **Trade Date** | Date the transaction occurred |
| **Settlement Date** | Date cash/security settled |
| **Instrument Code** | Stock symbol (e.g., SGJ, PBS) |
| **Transaction Type** | BUY, SELL, DIVIDEND, FUNDSIN, etc |
| **Transaction Category** | Auto-assigned by system |
| **Quantity** | Shares traded (SELL must be negative or system-signed) |
| **Price** | Price per share |
| **Fees** | Broker or exchange fees |
| **Cash Amount** | Cash movements not tied to trades |
| **Currency** | JMD, USD, etc |
| **FX to JMD** | Conversion rate at transaction date |
| **Notes** | Optional description |

Choose one:
• Manual single-entry input
• Bulk upload via formatted file

### ✍️ Option 1 — Manual Transaction Entry
Use the ➕ Add Transaction form.

Select the transaction type to get access to the appropriate form layout. To assist you, we have restricted specific fields to only those needed based on your transaction type.

✅ Supported Transaction Types
| Type | Description | Required Columns |
| :--- | :--- | :--- |
| **FUNDSIN** | Cash deposit | Trade Date, Currency, Fees, FX Rate, Amount
| **FUNDSOUT** | Cash withdrawal | Trade Date, Currency, Fees, FX Rate, Amount
| **BUY** | Security purchase | Trade Date, Currency, Quantity, Instrument, FX Rate, Price, Fees
| **SELL** | Security sale | Trade Date, Currency, Quantity, Instrument, FX Rate, Price, Fees
| **DIVIDEND** | Dividend received | Trade Date, Currency, Quantity, Instrument, FX Rate, Price, Fees
| **INTEREST** | Interest income | Trade Date, Currency, Fees, FX Rate, Amount
| **CAPITAL_RETURN** | Return of capital | Trade Date, Currency, Quantity, Instrument, FX Rate, Price, Fees
| **SPLIT** | Share splits | Trade Date, Currency, Quantity, Instrument, FX Rate, Price, Fees
| **BUYBACK** | Corporate repurchases | Trade Date, Currency, Quantity, Instrument, FX Rate, Price, Fees
| **FEES** | Account or trade fees | Trade Date, Currency, Fees, FX Rate, Amount
| **TAX** | Taxes | Trade Date, Currency, Fees, FX Rate, Amount

### 📂 Option 2 — Bulk Upload
We have prepared for your an Excel file template where each row will represent a transaction. We tried again to simplify for you the required inputs. Just below the Ledger Management Section, you will find a "Template" button. It will furnish you with a blank file with the minimum columns required for the **Portfolio Analytics Lab**.

✅ Required Columns
Your file must contain the following columns:

| Column Name | Explanation |
| :--- | :--- |
| **Trade_Date** | Dates must be in ISO or Excel date format (**YYYY-MM-DD**). |
| **Instrument_Code** | Refers to the asset's assigned ticker symbol. |
| **Transaction_Type** | Type in the transaction type from the "Supported Transaction Types" list. |
| **Quantity** | Enter the number of units/shares of the asset. |
| **Price** | Input the agreed settlement price per unit for the asset. |
| **Fees** | Input the total fees associated with the transaction for this asset. |
| **Cash Amount** | **ONLY** for: FUNDSIN, FUNDSOUT, INTEREST, FEES, TAX. Otherwise, leave blank. |
| **FX_to_JMD** | Use **1** for JMD assets; otherwise, use the exchange rate for 1 unit of FX. |
| **Notes** | Optional descriptive text for personal recollection. |

## ⚠️ Important Rules
• You do not need to be concerned with positive or negative signage, the engine were built to deduce and auto-signed transactions
• Cash movements like FUNDSIN, FUNDSOUT, FEES, INTEREST, and TAX go in Cash Amount 
• Trades transactions like BUY, SELL, DIVIDEND, CAPITAL RETURN, SPLIT require Quantity × Price. Do NOT use Cash Amount!
• FX_to_JMD must be supplied for non-JMD trades
• If an Instrument codes does not exist in the reference table such as a recent IPO, it will not be evaluated until its data is uploaded. 

### 🧠 How Portfolio Lab Uses Your Data

Your transactions drive a deterministic pipeline:

✔ Holdings reconstruction
✔ Historical valuation
✔ Time-weighted and money-weighted returns
✔ Performance attribution
✔ Risk modeling
✔ Market exposure diagnostics

If results look wrong, the cause is always upstream in the data.

📊 Navigating the App Pages
📂 Holdings

Verify:
• quantities
• cost basis
• market value

This page confirms your portfolio was reconstructed correctly.

📈 Performance & Attribution

View:
• TWRR / MWRR
• Asset and sector attribution
• Allocation vs selection effects

This explains where returns came from.

⚖️ Risk

Analyze:
• volatility
• diversification
• VaR (95% / 99%)
• risk contribution

This explains how risk is distributed.

🌍 Market Exposure

This page answers:

“What market forces does my portfolio actually respond to?”

You will see:
• Regression against market indices
• FX sensitivity
• Alpha and beta estimates
• Statistical diagnostics (R², p-values, residual tests)

⚠️ Important interpretation notes
• Low R² is common and expected in real portfolios
• Exposure ≠ prediction
• Results are diagnostic, not advice

This page is designed to help you reason about behavior, not forecast returns.

### 🧪 Best Practice
• Enter transactions chronologically
• Record cash deposits before trades
• Keep FX consistent
• Validate holdings before analyzing performance
• Use Market Exposure as a diagnostic, not a scorecard

🧩 Example of cash movement like FUNDSIN transaction 
```bash
Trade_Date: 2025-01-26  
Transaction_Type: FUNDSIN  
Cash_Amount: 100000  
Currency: JMD  
FX_to_JMD: 1  
```

🧩 Example BUY transaction
```bash
Trade_Date: 2025-01-26  
Instrument_Code: SGJ  
Transaction_Type: BUY  
Quantity: 1000  
Price: 55  
Currency: JMD  
FX_to_JMD: 1  
```

🧩 Example SELL transaction
```bash
Trade_Date: 2025-02-26  
Instrument_Code: SGJ  
Transaction_Type: SELL  
Quantity: -800  
Price: 60  
Currency: JMD  
FX_to_JMD: 1  
```

🧩 Example BUY transaction using USD asset
```bash
Trade_Date: 2025-01-26  
Instrument_Code: PBS  
Transaction_Type: BUY  
Quantity: 1000  
Price: 0.80  
Currency: USD  
FX_to_JMD: 156.50  
```

🧩 Example DIVIDEND transaction 
```bash
Trade_Date: 2025-01-26  
Instrument_Code: SGJ  
Transaction_Type: DIVIDEND  
Quantity: 1000  
Price: 0.10  
Currency: JMD  
FX_to_JMD: 1  
```

### 📌 Final Note

Portfolio Analytics Lab is deterministic.

If:
• transactions are correct
• FX is correct
• classifications are correct

Then the analytics are correct.

This platform is designed to help you understand portfolios as systems, not just collections of tickers.
