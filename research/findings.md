# User Research & Secondary Market Findings

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*

## 1. Primary Discovery Interviews (Anonymized Cohort Log)

Interviews conducted using the 10-question non-leading behavioral guide (`./interview-guide.md`) with first-time investors aged 18–25:

| Participant | Age & City | Apps Used | Understood `CNC` vs `MIS`? | Early Mistake / Friction Experienced | Uses Varsity? | Key Verbatim Insight |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Participant 1** | 21, Bengaluru (CS Student) | Kite, Groww | No | Bought Intraday (`MIS`) by mistake; position auto-squared off at a loss at 3:20 PM | Never heard of it | *"I thought MIS was cheaper because it asked for 5x less money."* |
| **Participant 2** | 22, Pune (Jr. Developer) | Kite | No | Sold a ₹400 `CNC` stock for a ₹8 gain; lost money after ₹15.93 DP charge | Only on YouTube | *"I didn't know selling a stock from Demat costs a flat ₹15+ fee even if brokerage is zero."* |
| **Participant 3** | 20, Delhi (Undergrad) | Kite, Upstox | Partially | Placed a `Market` order on a volatile mid-cap stock and bought at a spiked price | No | *"When I'm on the Buy screen, I Google what SL and SL-M mean in another tab."* |

---

## 2. Secondary Evidence & Public Data Triangulation

| Pain Point | Where It Happens | Public Evidence & Source | Severity |
| :--- | :--- | :--- | :--- |
| **1. `MIS` vs. `CNC` Jargon Trap** | Buy/Sell Order Pad (Top toggle) | Community discussions on [TradingQnA](https://tradingqna.com/) and `r/IndiaInvestments` detailing beginner confusion between Intraday (`MIS`) and Delivery (`CNC`). | **Critical** |
| **2. The "Cheap Stock" 5x Margin Illusion** | Buy/Sell Order Pad ("Margin Required" bar) | [Kite Google Play Store](https://play.google.com/store/apps/details?id=com.zerodha.kite3) reviews mentioning unexpected margin leverage and intraday losses. | **Critical** |
| **3. Surprise 3:20 PM Auto Square-Off Penalties** | Positions Tab & Console Contract Note | Zerodha charges ₹50 + 18% GST per order auto-squared off by the system at 3:20 PM ([Zerodha Charges](https://zerodha.com/charges/)). For small-budget users, this fee often exceeds the trade value. | **High** |
| **4. Order Type Confusion (`Market` vs `Limit` vs `SL`)** | Buy/Sell Order Pad (Type selector) | [Zerodha Support Docs](https://support.zerodha.com/) and forum posts from new users experiencing price slippage on Market orders or failing to set Stop-Loss (`SL`) triggers. | **Medium** |
| **5. Disconnected Education (The Varsity Silo)** | Across Kite Mobile App | [Zerodha Varsity](https://zerodha.com/varsity/) is hosted in a separate app/website with zero contextual links inside the Kite Order Pad. | **High** |
| **6. Delayed Fee Visibility (DP Charges)** | Order Pad & Next-Day Contract Note | Selling Delivery (`CNC`) stocks incurs a ₹13.5 + 18% GST Depository Participant (DP) charge ([Zerodha Pricing](https://zerodha.com/charges/)), wiping out gains on small ₹300–₹500 trades. | **High** |
| **7. Macro Retail Speculation Losses** | Market-wide | [SEBI's September 2024 Study](https://www.sebi.gov.in/) showed over 90% of individual F&O traders lost money, increasing regulatory scrutiny on broker interfaces. | **Critical** |

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*
