# Valuation Engine — Mathematical Specification
## 1. Purpose

The Valuation Engine computes a **point-in-time market valuation snapshot** of a portfolio, expressed in a common base currency (JMD), together with portfolio weights.

This engine is grounded in:

**Mark-to-Market Accounting** (FASB ASC 820 / IFRS 13 Fair Value Measurement)

**Portfolio Theory** (Markowitz, 1952)

**Multi-Currency Asset Pricing** (Solnik, 1974; Adler & Dumas, 1983)

It performs an **accounting valuation**, not a forecasting or pricing model.

## 2. Point-in-Time Price Selection

For each instrument 𝑖, we define:

$𝑃_𝑖(𝑡^∗)$ = Last observed market price on or before $𝑡^∗$

Formally:

$𝑃_𝑖(𝑡^∗)=𝑃_𝑖(𝜏)$ where $𝜏=max{𝑡\le𝑡^∗}$

This ensures **no look-ahead bias**, consistent with:
- Backtesting integrity standards
- Point-in-time data principles in empirical asset pricing

## 3. Cash Instrument Convention

For the synthetic instrument:

𝑖 = CASH

We define:

$𝑃_{CASH}=1$

This enforces:

$Market Value_{CASH}=𝑄_{CASH}$

This aligns with standard portfolio accounting conventions where base currency cash is already denominated in the reporting unit.

## 4. Missing Price Fallback (IPO / Illiquidity Handling)

If:

$𝑃_𝑖(𝑡^∗)$ is undefined and $𝑄_𝑖>0$

We set:

$𝑃_𝑖(𝑡^∗)=𝐶_𝑖$ Where: $𝐶_𝑖$= Unit Cost (book cost basis)

This is an **accounting fallback**, not a market estimate.

It avoids artificial portfolio collapse due to temporary price unavailability and preserves balance-sheet continuity.

## 5. FX Conversion Model

Let:

$𝑐_𝑖$ = instrument currency\
Base currency = JMD\
$𝐹𝑋_{𝑐→𝐽𝑀𝐷}(𝑡^∗)$ = average FX rate @ as of date

We define:\
$𝐹𝑋_𝑖=1$ , if $𝑐_𝑖$ = JMD,\
otherwise $𝐹𝑋_𝑖=𝐹𝑋_{𝑐_{𝑖}→𝐽𝑀𝐷}(𝑡^∗)$


This aligns with international portfolio valuation theory:

$𝑉_𝑖^{Base}=𝑉_𝑖^{Local}×𝐹𝑋_𝑖$

(See Solnik, 1974; Adler & Dumas, 1983)

## 6. Market Value Computation

### 6.1 Local Currency Value
$𝑀𝑉_𝑖^{Local}=𝑄_𝑖⋅𝑃_𝑖(𝑡^∗)$

Where:

$𝑄_𝑖$ = quantity held\
$𝑃_𝑖(𝑡^∗)$ = point-in-time price

### 6.2 Base Currency Value
$𝑀𝑉_𝑖^{JMD}=𝑀𝑉_𝑖^{Local}⋅𝐹𝑋_𝑖$

This converts all instruments into a unified numeraire.

## 7. Portfolio Aggregation
### 7.1 Total Portfolio Market Value

$𝑀𝑉_{Total}=\sum_{𝑖=1}^𝑁𝑀𝑉_𝑖^{JMD}$

This is a linear aggregation, consistent with:
- Modern Portfolio Theory (Markowitz, 1952)
- Linear pricing kernels under no-arbitrage assumptions

## 8. Portfolio Weights

If:

$𝑀𝑉_{Total}>0$

Then:\
$𝑤_𝑖=\frac{𝑀𝑉_𝑖^{JMD}}{𝑀𝑉_{Total}}$

Otherwise:

$𝑤_𝑖$ = 0

This produces a normalized weight vector:

$\sum_{𝑖=1}^𝑁𝑤_𝑖=1$

These weights form the basis for:
- Risk decomposition
- Attribution analysis
- Factor exposure modeling
- Beta estimation

## 9. Realized Gain Tracking (Structural Preservation)

Realized gain fields are preserved but not recomputed:

$Realized GL_𝑖^{JMD}$

The engine treats these as **state variables**, not valuation variables.

This reflects the separation between:
- Flow variables (realized gains)
- Stock variables (current market value)

## 10. Conceptual Framing

This engine implements:

1. Mark-to-Market Accounting (FASB ASC 820; IFRS 13)
2. Numeraire Transformation (Solnik, 1974 — International Asset Pricing)
3. Linear Portfolio Aggregation (Markowitz, 1952)

It does not:

- Forecast returns
- Estimate intrinsic value
- Model expected cash flows
- Apply discount rate modeling

It is a deterministic valuation snapshot.

11. Philosophical Position

Valuation is:

An accounting identity applied at a point in time.

Not:

- A belief system
- A predictive signal
- A risk model

**This engine answers:**

“What is the portfolio worth at $𝑡^∗$?”

**It does not answer:**

“What will it be worth?”
