# Experiment Design (A/B Test Specification)

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*

## 1. Hypothesis
If we show adaptive plain-English order labels and a pre-trade Sanity Check bottom sheet (**"Kite Training Wheels"**) to new users during their first 25 trades, then their **30-day healthy repeat trade rate** and **90-day retention** will increase because fewer beginners will suffer accidental intraday penalties or fee-shock losses.

## 2. Experiment Setup
* **Randomization Unit:** User-level (`user_id`) for newly activated Zerodha accounts placing their first mobile equity trade.
* **Control (50%):** Standard Kite Mobile Order Pad.
* **Variant (50%):** Kite Order Pad with "Kite Training Wheels" enabled for `< 25` lifetime trades.

## 3. Metrics
* **Primary Decision Metric (Fast-Feedback Proxy):** **30-Day Healthy Repeat Trade Rate** (`% of users completing >= 3 trades in their first 30 days with 0 auto-square-off penalties`).
* **Secondary Metric:** First-30-day 3:20 PM Auto-Square-Off incidence.
* **Guardrails:**
  1. *Order Pad Completion Rate* (`Orders Placed / Order Pad Opened`) must not drop by `> 2%`.
  2. *p99 Order Execution Latency* must show `0ms` statistically significant regression.

## 4. Sample Size & Duration Logic
* **Inputs:**
  * Control Baseline Rate: `40.0%` *(Assumption)*
  * Minimum Detectable Effect (MDE): `5% relative lift` (`40.0% -> 42.0%`)
  * Statistical Significance ($\alpha$): `95% confidence`
  * Statistical Power ($1 - \beta$): `80% power`
* **Required Sample Size:** **~9,500 users per arm (~19,000 users total)** *(Assumption based on standard two-proportion z-test)*.
* **Duration:** Assuming ~3,000 eligible first-time equity traders per day *(Assumption)*, we recruit users across **14 days** (to capture two full weekly market cycles) + **30 days** observation window = **6 weeks total duration**.

## 5. Ship / Iterate / Kill Decision Rule
* 🟢 **SHIP:** Primary Metric lifts by `>= +5% relative`, auto-square-off penalties drop by `>= 25%`, and Guardrails remain within bounds.
* 🟡 **ITERATE:** Auto-square-off penalties drop by `>= 30%`, *but* Order Pad Completion Rate drops by `> 5%` (indicating our bottom-sheet warning copy is too alarming). *Action:* Soften copy and test an inline banner vs. modal sheet.
* 🔴 **KILL:** Primary Metric shows `0%` or negative lift, and `> 50%` of users dismiss or disable the hints immediately.

## 6. Controlling for Market Volatility & Seasonality
Stock trading volume fluctuates heavily during macro events (Union Budget, RBI rate decisions, earnings season, market crashes).
1. **Strict Concurrent Testing:** Never use before-vs-after comparisons; run Control and Variant simultaneously so market volatility affects both groups identically.
2. **Stratified Sampling:** Balance IPO-acquired users and organic equity sign-ups equally across Control and Variant.
3. **Anomaly Blackouts:** Avoid starting experiment recruitment on outlier trading sessions (e.g., Budget Day or Diwali Muhurat Trading).

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*
