# 📈 Live US Stock Tracker

A Google Sheets-based dividend and stock portfolio tracker built for DIY investors. This tool monitors total return, dividend income, and portfolio performance with real-time data powered by `GOOGLEFINANCE()` and Google Apps Script.

---

## 🔗 Sheet for use 

**View the live tracker (read-only):**  
[🔗 RBC Stock Tracker – Google Sheets](https://docs.google.com/spreadsheets/d/1YDc308NsPFc7ON6ViP9ybww5p6fqjAQBWNQ0_xwZNSg/edit?usp=sharing)

---


## 🧩 Features

### 📊 Dashboard Overview
<img width="1389" height="675" alt="image" src="https://github.com/user-attachments/assets/c95bd0a5-6aed-4d65-a0a6-798a2210a1ef" />


- Real-time market data and historical 5-year price trends
- Portfolio breakdown by ticker and allocation
- Capital gains vs. dividend income visualized
- Monthly, yearly, and cumulative dividend tracking

### 🧾 Portfolio Positions – Automated RBC Integration
<img width="1280" height="221" alt="image" src="https://github.com/user-attachments/assets/65847795-68d0-4d2e-a0e1-d20188514cae" />

- Built to process **CSV transaction history exported from RBC Direct Investing**
- Just paste your raw data (Buys, Sells, Dividends, Deposits) into the `History` tab
- Google Apps Script parses and updates the **Positions** tab automatically
- Tracks:
  - Purchase cost, market value, and % gain
  - Total dividend income per security
  - Realized vs. unrealized gains
  - Auto-conversion to CAD (optional)
- Filters out "Cash" and non-equity rows for clean reporting


### 📄 Sample Transaction History
<img width="577" height="568" alt="image" src="https://github.com/user-attachments/assets/025f7da8-0d1c-4c13-968d-e55c7b572287" />

Handles data like this:

| Date       | Security | Action   | Quantity | Price   | Total   |
|------------|----------|----------|----------|---------|---------|
| 02/03/2021 | MSFT     | Buy      | 3        | 246.37  | 739.11  |
| 03/05/2021 | Cash     | Dividend | 1        | 4.59    | 4.59    |
| 03/08/2021 | MSFT     | Sell     | -3       | 226.68  | 680.05  |

---

## 🛠 Custom Apps Script
```javascript
function MyPortfolio(tickers, values) {
  var total = []
  var sums = {}

  for (i = 0; i < tickers.length; i++) {
    var t = tickers[i].toString()
    if (t != "Cash") {
      if (t in sums) {
        sums[t] += Number(values[i])
      } else {
        sums[t] = Number(values[i])
      }
    }
  }

  for (var ticker in sums) {
    if (sums[ticker] > 0) {
      total.push([ticker, sums[ticker]])
    }
  }

  return total
}
