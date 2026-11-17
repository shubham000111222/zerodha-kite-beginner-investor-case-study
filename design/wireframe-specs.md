# Low-Fidelity Wireframe Specifications

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*

## Screen 1: The Adaptive Order Pad (Beginner View)
*Active only for accounts with `< 25 trades`. Experienced accounts see the standard Kite Order Pad.*

    +---------------------------------------------------+
    |  <-  TATASTEEL                           ₹150.00  |
    |      NSE: ₹150.00  BSE: ₹149.95                   |
    +---------------------------------------------------+
    |  [Beginner Hint Active]          [Turn off hints] | <-- 1-tap opt-out for pros
    +---------------------------------------------------+
    |  Quantity: [ 2 ]              Price: [ 150.00 ]   |
    +---------------------------------------------------+
    |  PRODUCT                                     (?)  | <-- Tapping (?) opens Screen 3
    |  (•) Intraday (MIS)         ( ) Longterm (CNC)    |
    |  +---------------------------------------------+  |
    |  | 💡 Intraday (MIS): Borrow 5x margin. MUST   |  | <-- Plain-English Micro-Banner
    |  | sell before 3:20 PM today or pay ₹59 fee.   |  |     (Updates dynamically on tap)
    |  +---------------------------------------------+  |
    +---------------------------------------------------+
    |  TYPE                                        (?)  |
    |  ( ) Market   (•) Limit   ( ) SL   ( ) SL-M       |
    |  "Buys at ₹150.00 or lower. Safer than Market."   | <-- 1-line subtitle
    +---------------------------------------------------+
    |  Margin req: ₹60.00 (5x Leverage: Trade val ₹300) | <-- Explicitly shows true exposure
    |  Est. Round-trip Charges: ₹1.20 (Tap for details) |
    |                                                   |
    |  [  >>>>>>  SWIPE TO BUY  >>>>>>               ]  |
    +---------------------------------------------------+

---

## Screen 2: The Pre-Trade "Sanity Check" Bottom Sheet
*Triggers ONLY on a beginner's first 3 `MIS` orders ever, OR when estimated round-trip charges exceed `3%` of trade value.*

    +---------------------------------------------------+
    |  (Dimmed Order Pad in Background)                 |
    +===================================================+
    |  ⚠️  Quick Sanity Check (1 of 3 for new accounts) |
    |---------------------------------------------------|
    |  You selected INTRADAY (MIS) for 2 shares.        |
    |                                                   |
    |  1. 5x Leverage: You are paying ₹60, but taking   |
    |     price risk on ₹300 worth of shares.           |
    |  2. 3:20 PM Rule: If you don't sell by 3:20 PM    |
    |     today, our system auto-sells it for a         |
    |     ₹50 + GST penalty.                            |
    |                                                   |
    |  Want to hold this stock for more than 1 day?     |
    |                                                   |
    |  [  Switch to Longterm (CNC) — Pay ₹300  ] (Blue) | <-- Primary safe action
    |  [  Continue with Intraday (MIS) anyway  ] (Grey) | <-- Preserves user freedom
    |                                                   |
    |  [ ] Don't show this check again                  |
    +---------------------------------------------------+

---

## Screen 3: Inline "Varsity Bite" Drawer
*Opens as a native half-screen bottom sheet when tapping `(?)` so the user never loses their order form state.*

    +---------------------------------------------------+
    |  📘 Varsity Quick Guide                  [Close X]|
    |---------------------------------------------------|
    |  Market vs. Limit Orders (15-sec read)            |
    |                                                   |
    |  • Market Order: Buys immediately at whatever     |
    |    price a seller is asking. Fast, but risky if   |
    |    the stock price jumps suddenly.                |
    |                                                   |
    |  • Limit Order: You set the maximum price you     |
    |    are willing to pay. The order only executes    |
    |    at your price or better.                       |
    |                                                   |
    |  🔗 Want to learn more? [Save Chapter to Varsity] |
    +---------------------------------------------------+

> *Independent case study. Not affiliated with Zerodha. Feature ideas are illustrative and are not investment advice.*
