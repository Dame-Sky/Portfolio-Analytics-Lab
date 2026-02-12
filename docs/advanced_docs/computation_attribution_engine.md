# 🧮 Portfolio Analytics Lab — Attribution Engine
Mathematical Formulation & Attribution Theory

This document formalizes the mathematical foundations implemented in attribution_engine.py.

The attribution engine decomposes active portfolio return into interpretable components attributable to allocation decisions, security selection, and their interaction.

## 1. Attribution Framework

Portfolio Analytics Lab implements the classical Brinson attribution framework, supporting both:

- Brinson–Fachler (1985)
- Brinson–Hood–Beebower (1986)

These models explain the difference between portfolio and benchmark returns:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝑅_𝑝 − 𝑅_𝑏$ 
            

where:\
$R_p$ = portfolio return\
$R_b$ = benchmark return


### Primary References
Brinson, G., Fachler, N. (1985)
“Measuring Non-U.S. Equity Portfolio Performance”\
Brinson, G., Hood, L., Beebower, G. (1986)
“Determinants of Portfolio Performance”

## 2. Preconditions & Valuation Invariant

Attribution requires a non-zero starting portfolio valuation:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $$\sum_{i=1}^nMV_{i,0}>0$$

If this condition fails, attribution is mathematically undefined.

The engine enforces the valuation parity identity:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $V_1=V_0+(V_1−V_0)$

where:\
$V_0$ = starting portfolio value\
$V_1$ = ending portfolio value

This ensures attribution results reconcile to observed valuation changes.

## 3. Portfolio Weights and Returns
### 3.1 Portfolio Sector Weights

For sector i:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $$𝑊_{𝑝,𝑖}=\frac{𝑀𝑉_{𝑖,0}}{\sum_{𝑗=1}𝑀𝑉_{𝑗,0}}$$

where:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ${𝑀𝑉_{𝑖,0}}$ = sector market value at ${t_0}$

Weights satisfy:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $\sum_𝑖𝑊_{𝑝,𝑖}=1$

### 3.2 Portfolio Sector Returns

Sector-level portfolio return:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $$𝑅_{𝑝,𝑖}=\frac{𝑀𝑉_{𝑖,1}}{𝑀𝑉_{𝑖,0}}−1$$

**Cash Treatment**
Cash represents capital flow, not investment performance.

The engine explicitly enforces:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝑅_{𝑝,cash}=0$

This prevents artificial losses from deployed capital.

## 4. Benchmark Construction

Benchmarks are constructed dynamically using the investable universe implied by held securities.

4.1 Market-Capitalization Weights

For each benchmark constituent k:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝑀𝐶_{𝑘,0}=𝑃_{𝑘,0}×𝑆ℎ𝑎𝑟𝑒𝑠_𝑘$

Sector benchmark weights:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $$𝑊_{𝑏,𝑖}=\frac{\sum𝑀𝐶_{𝑘∈𝑖}}{\sum𝑀𝐶_𝑘}$$

4.2 Benchmark Sector Returns

Sector benchmark return:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $$𝑅_{𝑏,𝑖}=\frac{\sum_{𝑘∈𝑖}𝑀𝐶_{𝑘,0}⋅𝑟_𝑘}{\sum_{𝑘∈𝑖}𝑀𝐶_{𝑘,0}}$$

where:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $$𝑟_𝑘=\frac{𝑃_{𝑘,1}}{𝑃_{𝑘,0}}−1$$


## 5. Attribution Models

Let:\
$W_{p,i}$ = portfolio weight\
$W_{b,i}$ = benchmark weight\
$R_{p,i}$ = portfolio return\
$R_{b,i}$ = benchmark return


5.1 Brinson–Hood–Beebower (BHB)

Allocation Effect:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝐴_𝑖^{BHB}=(𝑊_{𝑝,𝑖}−𝑊_{𝑏,𝑖})⋅𝑅_{𝑏,𝑖}$ \
Selection Effect:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝑆_𝑖=𝑊_{𝑏,𝑖}⋅(𝑅_{𝑝,𝑖}−𝑅_{𝑏,𝑖})$ \
Interaction Effect:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝐼_𝑖=(𝑊_{𝑝,𝑖}−𝑊_{𝑏,𝑖})⋅(𝑅_{𝑝,𝑖}−𝑅_{𝑏,𝑖})$

5.2 Brinson–Fachler (BF)

First compute total benchmark return:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝑅_𝑏=\sum_𝑖𝑊_{𝑏,𝑖}⋅𝑅_{𝑏,𝑖}$

Allocation Effect (Relative):\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝐴_𝑖^{BF}=(𝑊_{𝑝,𝑖}−𝑊_{𝑏,𝑖})⋅(𝑅_{𝑏,𝑖}−𝑅_𝑏$

Selection and Interaction (Identical to BHB):\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝑆_𝑖=𝑊_{𝑏,𝑖}⋅(𝑅_{𝑝,𝑖}−𝑅_{𝑏,𝑖})$\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝐼_𝑖=(𝑊_{𝑝,𝑖}−𝑊_{𝑏,𝑖})⋅(𝑅_{𝑝,𝑖}−𝑅_{𝑏,𝑖})$


## 6. Total Attribution Identity

For each sector:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝑇_𝑖=𝐴_𝑖+𝑆_𝑖+𝐼_𝑖$

Portfolio-level identity:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $\sum_𝑖𝑇_𝑖=𝑅_𝑝−𝑅_𝑏$

This identity is exact, not approximate.

## 7. Security-Level Selection Attribution

Security-level attribution isolates pure stock selection.

7.1 Security Return:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $$𝑟_𝑘=\frac{𝑀𝑉_{𝑘,1}}{𝑀𝑉_{𝑘,0}}−1$$

7.2 Sector Benchmark Return:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $$𝑅_𝑠=\frac{\sum_𝑘𝑀𝑉_{𝑘,0}⋅𝑟_𝑘}{\sum_𝑘𝑀𝑉_{𝑘,0}}$$

7.3 Selection Contribution:\
Security weight:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $$𝑤_𝑘=\frac{𝑀𝑉_{𝑘,0}}{𝑉_0}$$

Selection contribution:\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $𝑆𝐶_𝑘=𝑤_𝑘⋅(𝑟_𝑘−𝑅_𝑠)$\
This removes allocation and sector effects.

## 8. Design Intent

The attribution engine is designed to be:

- mathematically closed
- benchmark-consistent
- cash-flow neutral
- auditable
- institutionally interpretable

It explains why a portfolio performed as it did — not how to predict future returns.

## 9. Final Note

Attribution is an **accounting decomposition**, not a forecasting model.
It does **not control for or estimate systematic risk (beta)**, nor does it assume that a portfolio and its benchmark share identical factor exposures.

Later, in the **Market Exposure module**, portfolio returns are explicitly regressed against market and macro factors, allowing you to observe how **true portfolio beta and factor sensitivities** influence the interpretation of performance.

It operates **after returns exist**, not before.

That distinction is what makes this engine reliable.
