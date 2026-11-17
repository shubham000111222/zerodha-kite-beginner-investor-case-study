# Zerodha Kite: Helping First-Time Investors Avoid Early Losses Without Slowing Down Pros

**A product management case study on designing zero-latency "Training Wheels" for beginner investors on Zerodha Kite.**

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*

---

## ⚡ TL;DR (2-Minute Recruiter Summary)
* **Problem:** First-time investors lose money and trust early because Kite’s Order Pad uses pro-trader abbreviations (`MIS`, `CNC`, `SL-M`) and exposes 5x intraday leverage by default—leading to accidental intraday trades, ₹50 + GST auto-square-off penalties at 3:20 PM `[SOURCE]`, and fee-shock on small orders.
* **Target User:** First-time Indian retail investors aged 18–25 starting with ₹5,000–₹10,000 per month *(Assumption)*.
* **Solution ("Kite Training Wheels"):** An adaptive, client-side UI layer active *only* for a user's first 25 executed trades. It adds plain-English order subtitles, inline 15-second Varsity cards, and a pre-trade "Sanity Check" bottom sheet using Zerodha's existing Nudge framework.
* **Key Metric:** **90-Day Healthy Active Beginner Rate** (Target: `+5% to +8%` relative lift *(Assumption)* with **0ms degradation** to p99 order execution latency).
* **My Role:** Solo PM & Technical Architect (Final-year CS student). Conducted user discovery, mapped the journey, prioritized via RICE, wrote the PRD, designed client-side zero-latency architecture, and structured the A/B test.

---

## 📑 Table of Contents
1. [Problem Statement & Why It Matters](#1-problem-statement--why-it-matters)
2. [User Research & Key Insights](#2-user-research--key-insights)
3. [User Persona & Journey Map](#3-user-persona--journey-map)
4. [Pain Points & RICE Prioritization](#4-pain-points--rice-prioritization)
5. [Solution Design: "Kite Training Wheels"](#5-solution-design-kite-training-wheels)
6. [PRD Summary & Technical Architecture](#6-prd-summary--technical-architecture)
7. [Wireframes](#7-wireframes)
8. [Metrics Framework](#8-metrics-framework)
9. [Experiment Design (A/B Test)](#9-experiment-design-ab-test)
10. [Risks, Trade-Offs & What I Did Not Build](#10-risks-trade-offs--what-i-did-not-build)
11. [What I'd Do Differently & Learnings](#11-what-id-do-differently--learnings)
12. [Links & Artifacts](#12-links--artifacts)

---

## 1. Problem Statement & Why It Matters
Zerodha is built for speed, reliability, and zero spam `[SOURCE]`. It charges ₹0 brokerage on Equity Delivery (`CNC`) and makes the vast majority of its revenue from flat ₹20 fees on Intraday (`MIS`) and F&O orders `[SOURCE]`.

However, when young beginners join Zerodha for its brand trust, they land in a cockpit built for experienced traders.

* **Why it matters for Users:** If a 22-year-old starting with ₹5,000 accidentally selects `MIS` (Intraday) instead of `CNC` (Long-term Delivery) because `MIS` asks for 5x less upfront margin, their stock gets forcefully sold at 3:20 PM with a ₹50 + 18% GST penalty `[SOURCE]`. They feel cheated and quit investing early.
* **Why it matters for Zerodha:**
  * **10-Year LTV:** A 22-year-old with ₹5,000 today becomes a 32-year-old investor with ₹20 Lakhs *(Assumption)*. Early churn destroys long-term Account Maintenance Charges (AMC), Coin mutual fund adoption, and trading LTV.
  * **Market Share Defense:** Competitors like Groww have overtaken Zerodha in active client count largely by capturing first-time investors with a simpler interface `[SOURCE]`.
  * **SEBI Regulatory Alignment:** SEBI studies show over 90% of individual F&O/speculative retail traders lose money `[SOURCE]`. Protecting beginners aligns with regulatory expectations.

---

## 2. User Research & Key Insights
* **Method:** 10-question behavioral interview guide ([view guide](./research/interview-guide.md)) targeting 8–10 beginner investors aged 18–25, supplemented by secondary analysis of Kite Google Play Store reviews, `r/IndiaInvestments`, and `TradingQnA` threads `[SOURCE]`.
* **Participants (Anonymized Sample):**
  * `Participant 1 (Age 21, Bengaluru - CS Student)`: Used Kite & Groww. Did not understand `CNC` vs `MIS`. Bought Intraday by mistake and got hit with auto-square-off charges. Quote: *"I thought MIS was cheaper because it asked for less money."*
  * *(See [./research/findings.md](./research/findings.md) for full participant log).*

**Top 3 Research Insights:**
1. **The "Cheap Stock" Margin Illusion:** Beginners see 5x lower "Margin Required" on `MIS` and assume it is a discount, unknowingly taking 5x leveraged intraday risk.
2. **Death by Small-Ticket Charges:** Beginners buying and selling ₹300–₹500 worth of shares in Delivery (`CNC`) don't realize that Depository Participant (DP) charges (₹13.5 + 18% GST on sell) `[SOURCE]` turn a ₹10 stock gain into a net loss.
3. **The Varsity Silo:** Zerodha owns India's best free financial education platform (*Varsity*), but zero Varsity context appears inside Kite where order decisions happen.

---

## 3. User Persona & Journey Map
### Target Persona: "First-Job Rohan" (Age 22, Bengaluru)
* **Monthly Savings:** ₹5,000 – ₹10,000 *(Assumption)*.
* **Core Jobs-To-Be-Done (JTBD):**
  1. *Execution Confidence:* Know what order type I am placing and what it costs before I swipe to buy.
  2. *Learning by Doing:* Understand terms like `CNC`, `MIS`, and `SL` in plain English without leaving the order screen.
  3. *Safe Exploration:* Avoid wiping out my small capital due to hidden leverage or cutoff rules.

![User Persona Placeholder](./assets/persona-rohan.png)
*Caption: Target Persona — First-Job Rohan (18–25 Beginner Segment).*

![Beginner Journey Map Placeholder](./assets/journey-map.png)
*Caption: Current 6-step beginner journey showing where excitement turns into loss shock at the Order Pad (Step 4) and 3:20 PM Auto Square-Off (Step 6).*

---

## 4. Pain Points & RICE Prioritization
I mapped 6 core pain points into "How Might We" (HMW) opportunities and scored them using **RICE** across an assumed monthly cohort of **100,000 new beginner accounts** *(Assumption)*. *(Full working in [./analysis/rice-scoring.md](./analysis/rice-scoring.md))*

| Opportunity (HMW) | Reach *(Assumption)* | Impact | Confidence | Effort (Person-Mos) | RICE Score |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1. Plain-English Order Pad Labels (Auto-off at 25 trades)** | 95,000 | 2.0 | 90% | 1.5 | **114,000 (Winner #1)** |
| **2. Pre-Trade "Sanity Check" Nudge (Leverage & Fee Alert)** | 80,000 | 3.0 | 85% | 2.5 | **81,600 (Winner #2)** |
| **3. Contextual Varsity Micro-Cards (`?` Drawer)** | 60,000 | 1.0 | 80% | 2.0 | **24,000 (Supporting)** |
| **4. Post-Trade Loss / Fee Breakdown** | 50,000 | 1.0 | 70% | 3.0 | 11,667 |
| **5. Full "Kite Lite" Beginner Mode UI** | 70,000 | 2.0 | 60% | 8.0 | 10,500 |
| **6. Paper Trading Sandbox inside Kite** | 40,000 | 1.0 | 50% | 10.0 | 2,000 |

---

## 5. Solution Design: "Kite Training Wheels"
* **Option A: Dual-Mode App ("Investor Mode" vs. "Trader Mode")** — *Rejected.* Violates Zerodha’s lean engineering culture by forcing maintenance of two UIs and hides the learning path.
* **Option B: "Kite Training Wheels" (Adaptive Labels + Pre-Trade Sanity Check + Varsity Bite)** — **SELECTED.** Active only for accounts with `< 25 lifetime executed orders`. Auto-graduates at trade #26 and has a 1-tap permanent opt-out for pros.
* **Option C: AI Copilot Stock Advisor** — *Rejected.* Severe SEBI compliance risk; recommending whether to buy/sell a stock crosses into unlicensed Investment Advisory (RIA/RA) `[SOURCE]`.

---

## 6. PRD Summary & Technical Architecture
📄 **[Read the Full One-Page PRD in `./analysis/prd.md`](./analysis/prd.md)**

* **Eligibility:** Default ON for users with `< 25 lifetime equity trades`. Can be toggled OFF in 1 tap on the Order Pad or in Settings.
* **Core Features:**
  1. **Dynamic Plain-English Subtitles:** Explains `Intraday (MIS)` (5x margin, 3:20 PM cutoff, ₹50+GST penalty) vs. `Longterm (CNC)` right under the toggle.
  2. **Pre-Trade "Sanity Check" Bottom Sheet:** Triggers *only* on a beginner's first 3 `MIS` orders OR when round-trip charges (including ₹15.93 DP charges) exceed `3%` of a small order value. Offers a 1-tap button to switch to `Longterm (CNC)`.
  3. **Inline "Varsity Bite" Drawer:** Tapping `(?)` opens a native 15-second card explaining `Market vs. Limit` orders without losing order state.
* **💻 Zero-Latency Architecture (CS Implementation):**
  * At login, the session payload caches `is_beginner_mode: true` (`lifetime_trades < 25`) in local device storage (`SharedPreferences`/`SQLite`).
  * Fee and break-even calculations are deterministic formulas executed **100% client-side in `< 0.2ms`** with **zero extra network API calls** during order placement.
* **SEBI Compliance:** Explains **100% order mechanics and statutory charges**, **0% stock quality or return predictions**.

---

## 7. Wireframes
| Screen 1: Adaptive Order Pad | Screen 2: Pre-Trade Sanity Check | Screen 3: Inline Varsity Bite |
| :---: | :---: | :---: |
| ![Screen 1 Placeholder](./assets/wireframe-1-order-pad.png) | ![Screen 2 Placeholder](./assets/wireframe-2-sanity-check.png) | ![Screen 3 Placeholder](./assets/wireframe-3-varsity-bite.png) |
| *Plain-English micro-banners for `<25 trades` + 1-tap pro opt-out.* | *Intercepts first 3 `MIS` trades with a 1-tap switch to `CNC`.* | *15-sec native bottom drawer explaining order types.* |

*(Full text wireframes and screen specs in [./design/wireframe-specs.md](./design/wireframe-specs.md))*

---

## 8. Metrics Framework
📄 **[Full Metrics & Retention Diagnosis in `./analysis/metrics-framework.md`](./analysis/metrics-framework.md)**

* **North Star Metric:** **90-Day Healthy Active Beginner Rate** (`% of new Day-1 trading users who execute >= 3 trades in their first 90 days AND place >= 1 trade between Day 61 and Day 90`). Target: `+5% to +8%` relative lift *(Assumption)*.
* **Input Metrics:** *Accidental Intraday Correction Rate* (Target: `25%–35%` *(Assumption)*), *First-30-Day Auto-Square-Off Incidence* (Target: `-40%` *(Assumption)*), *Varsity Bite Engagement Rate* (Target: `15%–20%` *(Assumption)*).
* **Guardrail Metrics:** *p99 Order Pad Render & Execution Latency* (**0ms degradation**), *Order Completion Rate* (must not drop by `> 2%`), *90-Day Cumulative Cohort Revenue & Support Ticket Rate*.

---

## 9. Experiment Design (A/B Test)
📄 **[Full A/B Test Math & Volatility Controls in `./analysis/ab-test-design.md`](./analysis/ab-test-design.md)**

* **Setup:** Concurrent 50/50 user-level A/B test on newly activated accounts placing their first mobile equity trade.
* **Primary Decision Proxy:** **30-Day Healthy Repeat Trade Rate** (`>= 3 trades in 30 days with 0 auto-square-off penalties`).
* **Sample Size:** Assuming a `40%` baseline *(Assumption)*, `5%` relative MDE (`40% -> 42%`), `95%` confidence, and `80%` power requires **~9,500 users per arm (~19,000 total)** over a **6-week window** (2 weeks recruitment + 30 days observation).
* **Handling Market Volatility:** Running Control and Variant **strictly concurrently** with stratified sampling (separating IPO sign-ups from organic equity sign-ups) ensures market dips hit both buckets equally.

---

## 10. Risks, Trade-Offs & What I Chose Not to Build
* **Revenue Trade-Off (Short-Term ₹20 vs. Long-Term LTV):** Steering beginners out of accidental Intraday (`₹20` fee) into Delivery (`₹0` fee) dips Day-1 brokerage slightly. We accept this trade-off because preventing early blow-ups preserves multi-year AMC, Coin, and active trader LTV.
* **Paternalism vs. Freedom:** Mitigated by auto-graduating after 25 trades, capping the bottom sheet at 3 `MIS` orders, and offering a 1-tap permanent opt-out.
* **What I Deliberately Did NOT Build:**
  1. ❌ *No Gamified Streaks or Badges* (encourages over-trading; dark pattern).
  2. ❌ *No Social/Community Copy-Trading Feed* (fuels FOMO and pump-and-dump risk).
  3. ❌ *No AI "Safe Stocks for Beginners" List* (violates SEBI advisory regulations).

---

## 11. What I'd Do Differently & Learnings
* **Limitation / Next Steps:** "Kite Training Wheels" prevents *operational execution mistakes* on the Order Pad, but beginners can still lose money by buying hyped penny stocks in Delivery (`CNC`). In V2, I would surface Zerodha's existing fundamental risk flags (high promoter pledging, ASM/GSM surveillance warnings) more clearly for beginners before they open the Order Pad.
* **Key Learning:** Building for fintech requires balancing three forces simultaneously—**user protection**, **unit economics**, and **SEBI regulatory boundaries**—while respecting engineering constraints like client-side latency.

---

## 12. Links & Artifacts
* 📊 **[Presentation Deck (8 Slides PDF)](./slides/zerodha-kite-case-study-deck.pdf)**
* 📋 **[Full One-Page PRD](./analysis/prd.md)**
* 📈 **[Metrics & Retention Drop Diagnosis](./analysis/metrics-framework.md)**
* 🧪 **[A/B Test Specification](./analysis/ab-test-design.md)**
* 👤 **Author:** Shubham — [GitHub](https://github.com/shubham000111222)

---

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*
