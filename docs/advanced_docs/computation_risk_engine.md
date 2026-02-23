# Risk Engine — Covariance, Volatility, and Value at Risk
## 1. Overview

The Risk Engine computes period-based portfolio risk using:

- Covariance matrix estimation
- Portfolio volatility
- Marginal risk contributions
- Component risk contributions
- Parametric (Gaussian) Value at Risk (VaR)

The framework is grounded in:

- Harry Markowitz (1952) — Modern Portfolio Theory
- Robert Merton (1972) — Continuous-time portfolio risk
- Philippe Jorion (2007) — Value at Risk methodology
- Euler Risk Decomposition Principle — homogeneous risk allocation

This engine performs statistical risk measurement, not forecasting.

## 2. Return Matrix Construction

Let:

$$P_{i,t}$$

be the price of asset 𝑖 at time 𝑡.

Discrete returns are computed as:

$$𝑟_{𝑖,𝑡}=\frac{𝑃_{𝑖,𝑡}}{𝑃_{𝑖,𝑡}}−1$$

This produces a return matrix:

$$𝑅∈𝑅^{𝑇×𝑁}$$

where:\
T = number of time observations\
N = number of assets

Assets with zero volatility (e.g., cash) are removed:

$$𝜎_𝑖=\sqrt{Var(𝑟_𝑖)}$$

If:

$$𝜎_𝑖=0$$

the asset is excluded to preserve covariance invertibility and prevent division errors.

## 3. Covariance Matrix Estimation

The sample covariance matrix is computed:

$$\sum=\frac{1}{𝑇−1}\sum^𝑇_{t=1}(𝑟_𝑡−\overline{𝑟})(𝑟_𝑡−\overline{𝑟})^⊤$$

Where:

$𝑟_𝑡$ is the vector of asset returns at time 𝑡\
$\overline{𝑟}$ is the sample mean return vector

This follows the classical Markowitz (1952) framework.

## 4. Portfolio Volatility

Let portfolio weights be:

$$𝑤=\begin{bmatrix} 𝑤_1\\\𝑤_2\\\⋮\\\𝑤_𝑁 \end{bmatrix}$$


where:

$$\sum_{𝑖=1}^𝑁𝑤_𝑖=1$$

Portfolio variance:

$$𝜎_𝑝^2=𝑤^⊤Σ_𝑤$$

Portfolio volatility (period-based):

$$𝜎_𝑝=\sqrt{𝑤^⊤Σ_𝑤}$$

Important:

This engine computes period volatility, not annualized volatility.
Annualization is handled externally if needed:

$$𝜎_{𝑎𝑛𝑛𝑢𝑎𝑙}=𝜎_𝑝\sqrt{𝑘}$$

where 
k is periods per year.

## 5. Marginal Risk Contribution (MRC)

Marginal Risk Contribution measures:

How much portfolio volatility changes with a marginal increase in weight $𝑤_𝑖$

Using calculus:

$$MRC_𝑖=\frac{∂𝜎_𝑝}{∂𝑤_𝑖}$$

For variance-based risk:

$$MRC_𝑖=\frac{(Σ𝑤)_𝑖}{𝜎_𝑝}$$

Vector form:

$$𝑀𝑅𝐶=\frac{Σ_𝑤}{𝜎_𝑝}$$

This is derived using properties of quadratic forms.

## 6. Component Risk Contribution (CRC)

Using Euler’s Homogeneous Function Theorem:

If risk is positively homogeneous of degree 1, then:

$$𝜎_𝑝=\sum_{𝑖=1}^𝑁𝑤_𝑖⋅MRC_𝑖$$

Component Risk Contribution:

$$CRC_𝑖=𝑤_𝑖⋅MRC_𝑖$$

Thus:

$$\sum_{𝑖=1}^𝑁CRC_𝑖=𝜎_𝑝$$

This ensures exact risk decomposition.

Reference: \
Tasche (2008), Capital Allocation to Business Units and Sub-Portfolios

## 7. Parametric Value at Risk (VaR)

Assuming returns are normally distributed:

$$𝑅_𝑝∼𝑁(𝜇_𝑝,𝜎_𝑝^2)$$

For confidence level α:

$$𝑧_𝛼=Φ^{−1}(𝛼)$$

where:

$Φ^{−1}$ is the inverse standard normal CDF. \
**i.e. The inverse standard normal cumulative distribution function (CDF), denoted as $Φ^{−1}$** , \
**is a function that takes a cumulative probability p (where 0 < p < 1 ) and returns the corresponding z-score** \
**such that the area under the standard normal curve to the left of z is exactly p.**

Portfolio VaR in return space:

$$VaR_𝛼=𝑧_𝛼⋅𝜎_𝑝$$

Converted to monetary terms:

$$VaR_𝛼^𝐽𝑀𝐷=𝑧_𝛼⋅𝜎_𝑝⋅𝑉_𝑝$$

where:

$𝑉_𝑝$ = portfolio market value in JMD

**Interpretation**: \
With confidence level 𝛼, the expected loss will not exceed this amount over the selected period, assuming normality.

Reference: \
Jorion (2007), Value at Risk

## 8. Component VaR Allocation

Since VaR is homogeneous of degree 1 under Gaussian assumptions, we can allocate VaR proportionally:

$$CVaR_𝑖=\frac{CRC_𝑖}{𝜎_𝑝}⋅VaR_𝛼^{𝐽𝑀𝐷}$$

Thus:

$$\sum_{𝑖=1}^𝑁CVaR_𝑖=VaR_𝛼^{𝐽𝑀𝐷}$$

This preserves additivity.

## 9. Mathematical Properties
1. Linearity of Variance Operator

$$Var(𝑎𝑋+𝑏𝑌)=𝑎^2Var(𝑋)+𝑏^2Var(𝑌)+2𝑎𝑏Cov(𝑋,𝑌)$$

Knowing the linearity of variance allows you to calculate the risk of a complex system by looking at its individual parts. 
However, Variance is not actually linear. Variance is quadratic. This "non-linearity" is exactly why it is so useful to understand.
Here is why knowing this relationship is essential:

a. The "Free Lunch" of Diversification
If variance were perfectly linear, adding more stocks to a portfolio wouldn't help you; the risk would just be the sum of the parts.
Because of the 2abCov (X, Y) term, you can actually lower your total risk without lowering your expected return.
If you pick two assets that move in opposite directions (negative covariance), they "cancel out" each other's volatility.
Understanding this formula allows you to mathematically prove that a diversified portfolio is objectively safer than a single-asset bet.

b. Error Propagation (Science & Engineering)
If you are building a bridge or a circuit, every component has a small "margin of error" (variance).
Knowing this formula helps you calculate the total uncertainty of the final product.
If the components are independent (Cov=0), the formula simplifies to a2Var(X) + b2Var(Y).
This tells engineers that the "total error" is often smaller than the "sum of the maximum possible errors," allowing for more efficient designs.

c. Scaling and Volatility (The "Square Root" Rule)
The a2 and b2 in the formula are critical. They tell you that if you double your bet (a = 2), you don't double your risk—you quadruple it (since 22=4).
This is why standard deviation (the square root of variance) is often preferred for daily use: it brings the "risk" back down to the same scale as the "weight."
Knowing the math prevents you from underestimating how quickly risk grows when you increase your exposure.

d. Data Processing (Signal vs. Noise)
In data science, when you aggregate multiple noisy sensors to get a single reading:
The signals (the parts that are the same) add up linearly.
The noise (the random variance) adds up at a different rate (often the square root of the sum of variances).
By understanding this formula, you can calculate the Signal-to-Noise Ratio (SNR) and determine how many sensors you need to get a "clean" average.


2. Positive Homogeneity

$$𝜎(𝜆𝑤)=∣𝜆∣𝜎(𝑤)$$

Positive Homogeneity means that your risk scales exactly with the size of your bet. If you double your investment in 
a specific stock, you double your risk for that position. 

a. In portfolio analytics, this property offers several key advantages: \
Predictable Scaling: It simplifies calculations by ensuring that risk is proportional to size. If you know the risk of a 
$1,000 position, you automatically know the risk of a $10,000 position without needing a new complex model.

b. Currency Neutrality: It ensures that your risk assessment doesn't change just because you change the units of measurement. 
Whether you measure your portfolio in Dollars, Euros, or Yen, the underlying risk remains consistent once converted.

c. Capital Allocation: It allows for "Euler Allocation," a method where the total risk of a portfolio can be perfectly broken 
down and "blamed" on its individual components. Because risk scales linearly, you can precisely calculate how much each 
asset contributes to the total.

d. No "Free" Diversification from Size Alone: It forces the model to recognize that simply buying more of the same thing doesn't 
provide any diversification. To reduce risk, you must add different types of assets (subadditivity), not just larger quantities of existing ones. 



3. Exact Decomposition

$$𝜎_𝑝=\sum_iCRC_𝑖$$

## 10. Model Assumptions & Limitations

1. Returns are approximately normally distributed
2. Covariance is stable over the selected window
3. No regime shifts within the period
4. Liquidity and transaction costs are ignored
5. Tail risk beyond Gaussian assumption is not modeled

This is a parametric risk model, not a stress-testing engine.

## 11. Philosophical Positioning

This risk engine follows the Markowitz mean-variance paradigm, where:
- Risk is defined as dispersion (variance)
- Correlation drives diversification
- Risk is decomposable using Euler principles

It does not attempt:
- Expected shortfall modeling
- GARCH volatility forecasting
- Regime-switching models
- Fat-tail corrections

It is a classical, transparent, mathematically consistent risk framework suitable for:
- Portfolio diagnostics
- Risk budgeting
- Exposure decomposition
- Institutional-style reporting
