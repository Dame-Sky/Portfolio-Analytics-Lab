# Computation Engine – Performance & Return Mathematics

This document explains the **mathematical logic** underlying the `returns_engine.py`. The goal is not to describe code mechanics, but to formalize the **financial theory**, equations, and intellectual lineage that inform the implementation.

---

## 1. Philosophical Foundation

The returns engine is grounded in **Time-Weighted Rate of Return (TWRR)** methodology, as codified by:

* **CFA Institute – GIPS® Standards**
* Dietz (1966), Sharpe (1966)
* Bacon (2008), *Practical Portfolio Performance Measurement and Attribution*

The governing philosophy is:

> *Performance measurement must isolate the effect of investment decisions from the effect of external cash flows.*

This engine therefore **explicitly removes the impact of contributions and withdrawals**, measuring only market-driven performance.

---

## 2. External vs Internal Cash Flows

Let:

* $( V_t )$ = portfolio market value at valuation date ( t )
* $( F_t )$ = **net external cash flow** during period $( (t_1, t] )$

External flows include:

* Capital contributions (FUNDSIN)
* Capital withdrawals (FUNDSOUT)

Trades (BUY/SELL) are **not** external flows because capital remains inside the portfolio.

---

## 3. First-Period Normalization (Initial Capital Problem)

Classical TWRR assumes an existing portfolio. In real portfolios, the **first valuation often coincides with the initial contribution**.

To align with CFA guidance, the first period return is defined as:

$$R_1 = \frac{V_1}{|F_1|} - 1$$


Where:

* $( F_1 )$ is the initial contribution
* $( V_1 )$ is the first observed market value

This avoids undefined denominators and ensures consistency with industry practice.

---

## 4. Period Return Formula (Subsequent Periods)

For periods $( t \ge 2 )$, the period return isolates market movement by **removing external flows from the numerator**.

### Core Equation

$$R_t = \frac{V_t - F_t}{D_t} - 1$

Where the denominator $( D_t )$ depends on the **timing logic of flows**.

---

## 5. Denominator Logic – Capital at Risk

The key conceptual decision is defining **capital at risk**.

Let:

* $( V_{t-1} )$ = portfolio value at previous valuation

### Case 1: Contribution $( ( F_t > 0 ) )$

New capital was available to earn returns:

$D_t = V_{t-1} + F_t$

### Case 2: Withdrawal $( ( F_t < 0 ) )$

Capital was present **for most of the period**, so the denominator remains unchanged:

$D_t = V_{t-1}$

This treatment aligns with **true TWRR logic**, avoiding the common error of penalizing returns for withdrawals.

---

## 6. Geometric Linking (Cumulative Return)

Time-weighted returns are **geometrically linked**:

$R_{\text{cum}} = \prod_{t=1}^{T} (1 + R_t) - 1$

This ensures:

* Path independence
* Comparability across portfolios
* Consistency with benchmark indices

---

## 7. Time Normalization

Let:

* $( D_t )$ = number of days since inception

### Projected Annualized Return (UI Approximation)

Used for early-stage visualization:

$R_{\text{proj}} = (1 + R_{\text{cum}})^{\frac{365.25}{D_t}} - 1$

This is **not GIPS-compliant**, but provides intuitive scaling for short histories.

---

## 8. CFA-Compliant Annualization

CFA standards permit annualization **only when the measurement period exceeds one year**:

$R_{\text{CFA}} = (1 + R_{\text{cum}})^{\frac{365.25}{D_t}} - 1 \quad \text{if } D_t \ge 365$

Otherwise, annualized returns are suppressed to avoid misleading extrapolation.

---

## 9. Why This Matters

This computation engine ensures:

* Contributions do not inflate skill
* Withdrawals do not distort risk
* Performance reflects **investment decisions only**

In short:

> **This engine measures how well capital was managed — not how much capital was added.**

---

## 10. References

* CFA Institute (2020). *Global Investment Performance Standards (GIPS®)*
* Bacon, C. (2008). *Practical Portfolio Performance Measurement and Attribution*
* Dietz, P. (1966). *Components of a Measurement Model*
* Sharpe, W. (1966). *Mutual Fund Performance*
