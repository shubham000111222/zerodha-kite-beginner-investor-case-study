# Product Requirements Document (PRD): Kite Training Wheels

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*

* **Feature Name:** Kite Training Wheels
* **Platform:** Kite Mobile App (Android & iOS)
* **Status:** Draft Proposal | **Target Segment:** First-time investors (`< 25 lifetime executed trades`)

---

## 1. Problem & Goal
* **Problem:** Beginners (`< 25 trades`) lose money and trust early because pro-trader abbreviations (`MIS`, `CNC`, `SL-M`) and hidden fee mechanics on small orders cause accidental leveraged trades, ₹50 + GST auto-square-off penalties at 3:20 PM `[SOURCE]`, and net-negative returns due to ₹13.5 + GST DP charges `[SOURCE]`.
* **Goal:** Reduce accidental intraday orders, auto-square-off penalties, and 90-day beginner churn while adding **0ms of latency** for experienced users.

## 2. Eligibility & Graduation Logic
* Enabled by default when `user.lifetime_executed_equity_orders < 25`.
* Automatically turns off at trade #26 with a dismissible toast notification.
* Can be permanently turned OFF in 1 tap via the Order Pad header (`"Turn off hints"`) or toggled anytime in `Settings -> Order Preferences`.

## 3. User Stories
1. **Plain-English Subtitles:** *As a beginner on the Order Pad*, I want to see a 1-line explanation under `Intraday (MIS)` and `Longterm (CNC)` so I know which option holds the stock overnight and which option auto-sells at 3:20 PM.
2. **Upfront Small-Order Fee Guard:** *As a small-budget beginner placing an order under ₹500*, I want to see if round-trip statutory and DP charges exceed `3%` of my order value *before* I swipe to buy.
3. **Pre-Trade Sanity Check:** *As a beginner selecting Intraday (`MIS`) for the first 3 times*, I want a quick confirmation card explaining the 5x leverage exposure and ₹50 + GST auto-square-off rule, with a 1-tap button to switch to `Longterm (CNC)`.
4. **Inline Varsity Bite:** *As a curious beginner*, I want to tap `(?)` next to order types (`Market`, `Limit`, `SL`) to read a 15-second native summary without leaving my order ticket.

## 4. Scope
* **IN SCOPE:** Mobile Order Pad adaptive subtitles, pre-trade Sanity Check bottom sheet (capped at first 3 `MIS` orders or when fees `> 3%` of trade value), native Varsity Bite bottom drawer, 1-tap permanent opt-out.
* **OUT OF SCOPE:** Stock quality ratings or buy/sell recommendations (prohibited by SEBI `[SOURCE]`), Kite Web changes (Phase 1 is Mobile only), any UI changes for accounts with `>= 25` trades.

## 5. Edge Cases
* **Pro User on a New Account:** If an experienced trader opens a family account, they can tap `"Turn off hints"` on their very first order screen in 1 tap.
* **Small Screens (5-inch phones):** The 2-line helper banner collapses into 1 line when the numeric keyboard opens so the `"Swipe to Buy"` button is never pushed off-screen.
* **Fast-Moving Markets:** The Sanity Check bottom sheet includes a `"Don't show this check again"` checkbox and automatically stops after 3 `MIS` trades.

## 6. Technical Architecture (Zero-Latency Design)
* **Local Session Flag:** On login, the user profile payload includes `is_beginner_mode: boolean` (evaluated asynchronously on backend via `lifetime_trades < 25`) and caches it in local device storage (`SharedPreferences` / `SQLite`).
* **Client-Side Fee Math:** Brokerage, STT, exchange fees, and DP charge estimates are deterministic mathematical formulas computed **100% on the phone CPU in `< 0.2ms`**.
* **Zero Extra Network Calls:** Varsity micro-card text and fee parameters are synced via a lightweight remote config JSON on app launch, ensuring **0 network API calls** when opening or swiping the Order Pad.

## 7. SEBI Regulatory Compliance
* Under SEBI Investment Adviser (IA) and Research Analyst (RA) regulations `[SOURCE]`, brokers cannot offer unapproved buy/sell advice on specific securities.
* All copy in "Kite Training Wheels" strictly describes **exchange rules, order mechanics, and statutory charges**—never the underlying stock's prospects.

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*
