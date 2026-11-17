# Metrics Framework & Retention Drop Diagnosis

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*

## 1. North Star, Input & Guardrail Metrics

| Category | Metric Name | Formula / Calculation | Target *(Assumption)* |
| :--- | :--- | :--- | :--- |
| **North Star** | **90-Day Healthy Active Beginner Rate** | `% of Day-1 trading beginners who execute >= 3 trades in 90 days AND place >= 1 trade in Days 61–90` | **+5% to +8% relative lift** |
| **Input 1** | **Accidental Intraday Correction Rate** | `Beginners clicking "Switch to CNC" / Total beginners shown Sanity Check` | **25% – 35% switch rate** |
| **Input 2** | **First-30-Day Auto-Square-Off Incidence** | `% of <30-day users hit with a 3:20 PM system auto-square-off penalty` | **-40% reduction** |
| **Input 3** | **Varsity Bite Engagement Rate** | `Unique beginners tapping (?) / Unique beginners opening Order Pad` | **15% – 20% engagement** |
| **Guardrail 1** | **Order Pad Render & Execution Latency (p99)** | `99th percentile time (ms) from tapping Buy to Order Pad render & exchange API call` | **0ms degradation** |
| **Guardrail 2** | **Pro-User False Positive Opt-Out Rate** | `% of users clicking "Turn off hints" within 5 seconds of trade #1` | **Track only** |
| **Guardrail 3** | **Beginner Support Ticket Rate** | `Tickets on "charges / auto square-off" from <30d users / Total <30d trading users` | **-25% reduction** |
| **Guardrail 4** | **90-Day Cumulative Revenue per Beginner** | `Total Brokerage + AMC earned per beginner cohort over 90 days` | **Neutral at D30; Positive by D180** |

---

## 2. Diagnostic Exercise: "Beginner 90-Day Retention Dropped 10% QoQ"

### Step 1: Clarify Metric & Scope
* Confirm definition of "Beginner" (age 18–25 vs. first-time traders) and "Retention" (app open vs. executed trade in Days 61–90).
* Check if the 10% drop is **relative** (e.g., 40% to 36%) or **absolute** (40% to 30%), and whether it was gradual or sudden.

### Step 2: Check Data Quality
* Verify that recent Kite Flutter updates on Android/iOS did not break telemetry for `order_executed` or `session_start`.

### Step 3: Segment the Cohort
* **Acquisition Intent:** Separate users whose first action was applying for a hyped **IPO** vs. buying an equity stock.
* **First Asset Class:** Delivery (`CNC`) vs. Intraday (`MIS`) vs. F&O.
* **Device & Ticket Size:** Android vs. iOS; starting capital `< ₹2,000` vs. `> ₹10,000`.

### Step 4: External vs. Internal Causes
* **External:** Nifty 50 market correction causing red portfolios ("ostrich effect"), SEBI margin/lot-size rule changes ([Source](https://zerodha.com/charges/)), or competitor zero-brokerage campaigns.
* **Internal:** UPI payment gateway failures during repeat funding, UI changes, or support delays.

### Step 5: Ranked Hypotheses & Action Plan

| Rank | Hypothesis | Data to Confirm | Recommended Action |
| :--- | :--- | :--- | :--- |
| **#1** | **"IPO Tourist" Cohort Effect:** 90 days ago, a mega-IPO brought in 2x more low-intent beginners who churned after failing to get IPO allotment. | Compare 90-day retention of *"1st action = IPO bid"* vs. *"1st action = Stock buy"*. | Separate IPO cohorts in reporting; trigger a post-IPO educational nudge introducing Index ETFs or Coin SIPs once UPI funds unblock. |
| **#2** | **Market Pullback / Early Loss Shock:** A broader market dip caused 70%+ of last quarter's beginners to lose money on their first 3 trades. | Correlate cohort Day-14 PnL with Day-90 retention across quarters. | Ship "Kite Training Wheels" and send a non-pushy Varsity digest on market cycles. |
| **#3** | **Repeat UPI Funding Friction:** Beginners invested their initial ₹1,000 and hit bank failures when adding funds in Month 2. | Check conversion funnel of `Add Funds -> UPI Redirect -> Payment Success` by bank. | Route default UPI traffic through the highest-success Payment Service Provider (PSP). |

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*
