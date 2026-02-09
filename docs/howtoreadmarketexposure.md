#### 📘 How to Read Market Exposure Results

A Practical Guide for Portfolio Analytics Lab

## 🎯 What This Page Is (and Is Not)

The Market Exposure page answers one question:

“What systematic forces has my portfolio historically responded to?”

It does not:

- Predict future returns
- Score portfolio quality
- Replace fundamental analysis
- Guarantee statistical certainty

Think of this page as a diagnostic X-ray, not a crystal ball.

## 🧠 The Big Idea

Your portfolio’s excess returns are regressed against external factors such as:

- Market index returns
- FX movements
- Risk-free rate adjustments

The goal is to explain behavior, not maximize fit.

## 📈 The Core Regression Model (Conceptually)

Portfolio Lab estimates a model of the form:
```bash
Portfolio Excess Return
= Alpha
+ β₁ × Market Excess Return
+ β₂ × FX Change
+ error
```

Each term answers a different question.

### 🧩 Understanding Each Output
## 1️⃣ Alpha (Intercept)

What it asks:

**Did the portfolio outperform after accounting for market and FX exposure?**

How to read it:

- Positive & significant → evidence of unexplained excess return
- Insignificant → performance mostly explained by known factors
- Negative → underperformance after adjustment

⚠️ Important:
Insignificant alpha is normal, especially over short horizons.

## 2️⃣ Market Beta

What it asks:

**How sensitive is my portfolio to the market?**

Interpretation:

- β ≈ 1.0 → market-like behavior
- β > 1.0 → amplified market exposure
- β < 1.0 → defensive behavior

Statistical significance matters more than size.

## 3️⃣ FX Sensitivity

What it asks:

**Does currency movement materially affect my portfolio?**

Key insight:

- FX exposure often exists even in “local” portfolios
- Insignificant FX does not mean “no FX risk”
- It means FX hasn’t systematically explained returns so far

## 4️⃣ R-Squared (R²)

This is the most misunderstood metric.

What it actually means:

**What percentage of return variance is explained by this model?**

Typical ranges in real portfolios:

- 10–30% → common
- 30–50% → strong
- 50%+ → rare outside index funds

## 🚨 Low R² is NOT failure

It usually means:

- Stock selection matters
- Idiosyncratic risk is dominant
- Portfolio is not a simple factor bet

That’s often a good thing.

## 5️⃣ F-Statistic & Prob(F)

What it asks:

**Does the model explain anything at all?**

How to read it:

- Low Prob(F) (< 0.05) → model is statistically meaningful
- High Prob(F) → factors do not jointly explain returns
- This is a model validity check, not a performance grade.

## 6️⃣ P-Values (Individual Factors)

What they ask:

**Is this factor consistently related to returns?**

Rules of thumb:

- p < 0.05 → statistically significant
- p > 0.10 → noisy or unstable relationship

⚠️ Significance ≠ importance
⚠️ Insignificance ≠ irrelevance

Markets are noisy by nature.

## 🛡️ Model Integrity Diagnostics

These protect you from false confidence.

🔹 Durbin–Watson

Checks for autocorrelation.

- ~2.0 → healthy
- <1.5 or >2.5 → potential issues

🔹 Condition Number

Checks multicollinearity.

- <1000 → acceptable
- Very high values → coefficients may be unstable

🔹 Residual Normality Tests

Look for extreme skew or fat tails.

Minor deviations are normal in financial returns.

## 📉 Attribution Breakdown (Below the Regression)

This section translates coefficients into economic contribution:

- Risk-free return contribution
- Market beta contribution
- FX contribution
- Residual (Jensen’s Alpha)

⚠️ Large negative residuals often reflect:

- Timing effects
- Volatility clustering
- Short sample periods

Not necessarily “bad management.”

## 🧠 How to Think About Low R² + Significant Beta

This is a very common and healthy result.

It means:

- Market matters
- But it does not dominate
- Portfolio behavior is partly structural, partly idiosyncratic

This is typical of:

- Concentrated portfolios
- Dividend-driven strategies
- Emerging market equities

## 🧪 What This Page Is Best Used For

✅ Understanding portfolio behavior
✅ Stress-testing intuition
✅ Comparing exposure across portfolios
✅ Diagnosing unintended risk

❌ Ranking portfolios
❌ Forecasting returns
❌ Marketing performance

## 📌 Final Takeaway

A good Market Exposure result is **honest**, not impressive.

Low R² + meaningful diagnostics often means:

You are analyzing a real portfolio, not a textbook factor model.

That is exactly the point.
