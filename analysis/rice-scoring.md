# Opportunity Prioritization (RICE Framework)

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*

## Formula
$$\text{RICE Score} = \frac{\text{Reach} \times \text{Impact} \times \text{Confidence}}{\text{Effort}}$$

* **Cohort Baseline:** 100,000 new beginner accounts per month *(Assumption)*.
* **Impact Scale:** `0.25` (Minimal), `0.5` (Low), `1.0` (Medium), `2.0` (High), `3.0` (Massive).
* **Effort:** Engineering and design person-months.

## RICE Scoring Table

| Opportunity ("How Might We...") | Reach *(Assumption)* | Impact | Confidence | Effort (Person-Mos) | Working & Final RICE Score |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **HMW #1: Plain-English Order Pad Labels** *(Auto-off after 25 trades)* | **95,000** *(95% of active beginners open Order Pad)* | **2.0** *(Directly stops MIS vs CNC confusion)* | **90%** | **1.5** *(Simple frontend label & toggle)* | `(95,000 × 2 × 0.9) / 1.5` = **114,000 (Rank 1)** |
| **HMW #2: Pre-Trade "Sanity Check" Nudge** *(Leverage & Fee Alert)* | **80,000** *(80% attempt small-ticket or intraday orders)* | **3.0** *(Prevents direct loss & ₹50 penalty)* | **85%** | **2.5** *(Extends existing Nudge engine)* | `(80,000 × 3 × 0.85) / 2.5` = **81,600 (Rank 2)** |
| **HMW #3: Contextual Varsity Micro-Cards** | **60,000** *(60% tap info icons)* | **1.0** *(Improves knowledge; indirect impact)* | **80%** | **2.0** *(Content mapping + bottom drawer)* | `(60,000 × 1 × 0.8) / 2.0` = **24,000 (Rank 3)** |
| **HMW #5: Post-Trade Loss / Fee Breakdown** | **50,000** *(50% lose money or pay high % fees on Trade 1)* | **1.0** *(Reactive—after money is already lost)* | **70%** | **3.0** *(Requires Console back-office sync)* | `(50,000 × 1 × 0.7) / 3.0` = **11,667 (Rank 4)** |
| **HMW #4: Full "Kite Lite" Beginner Mode** | **70,000** *(70% opt-in during onboarding)* | **2.0** *(Clean UI, but hides growth path)* | **60%** | **8.0** *(Heavy effort maintaining two UIs)* | `(70,000 × 2 × 0.6) / 8.0` = **10,500 (Rank 5)** |
| **HMW #6: Paper Trading Sandbox inside Kite** | **40,000** *(40% try virtual trading)* | **1.0** *(Users take fake risks with fake money)* | **50%** | **10.0** *(Backend load + SEBI rules)* | `(40,000 × 1 × 0.5) / 10.0` = **2,000 (Rank 6)** |

## Justification for Final Selection
We combine **HMW #1** and **HMW #2** (supported by lightweight links from **HMW #3**) into **"Kite Training Wheels"**:
1. **Prevention Beats Cure:** Intercepting an accidental 5x leveraged `MIS` trade *before* execution saves user capital immediately.
2. **Respects Zerodha's Lean Tech Philosophy:** Avoids maintaining a bloated "Kite Lite" fork and respects SEBI restrictions on gamified virtual/paper trading `[SOURCE]`.

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*
