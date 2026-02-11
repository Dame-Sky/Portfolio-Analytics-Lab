# 🧠 Portfolio Analytics Lab — Technical Documentation

This document describes the internal architecture of Portfolio Analytics Lab and how its engines collaborate to reconstruct portfolios, compute performance, and evaluate risk.

---

## 🏗️ Architecture Overview

Portfolio Lab is built as a **multi-engine financial analytics system**.

Each engine performs a single institutional-grade responsibility:

| Engine | Purpose |
| :--- | :--- |
| **holdings_engine** | Reconstructs portfolio positions |
| **valuation_engine** | Prices positions through time |
| **returns_engine** | Computes portfolio returns |
| **sector_engine** | Sector grouping & attribution |
| **attribution_engine** | Performance attribution |
| **risk_engine** | Risk and decomposition analytics |
| **exposure_engine** |	Market, FX, and factor exposure diagnostics |
| **ui_components** |	Shared presentation & diagnostics primitives |

The Streamlit UI only orchestrates these engines.

---

# 🔁 Data Flow

```bash
Transactions
    ↓
Holdings Engine
    ↓
Valuation Engine
    ↓
Returns Engine
    ↓
Attribution Engine
    ↓
Risk Engine
    ↓
Simulation Engine
    ↓
Market Exposure Engine
```
Each layer consumes the previous layer’s output.


# 🧮 holdings_engine.py
Responsible for position reconstruction.

Core responsibilities:\
• Roll forward quantities\
• Apply signed trade logic\
• Handle non-position transactions\
• Aggregate holdings per date

Key functions:\
build_holdings_snapshot(...)\
Builds positions as-of a specific date.

Output:

Instrument_Code | Quantity | Cost_Basis | Currency | Sector\
build_holdings_history(...)\
Builds time series of holdings.

Used by valuation and performance engines.

# 💰 valuation_engine.py\
Responsible for market valuation.

Core responsibilities:\
• Attach latest or historical prices\
• Normalize FX\
• Compute market value\
• Create valuation snapshots

Key functions:
build_valuation_snapshot(...)

Output:

Instrument | Quantity | Market_Price | FX | Market_Value_JMD\
build_valuation_history(...)\
Output:

Date | Instrument | Market_Value_JMD\
This becomes the backbone of return computation.

# 📈 returns_engine.py
Responsible for portfolio return mathematics.

Core responsibilities:\
• Separate market movement from cash flows\
• Compute sub-period returns\
• Chain-link TWRR\
• Support future MWRR/IRR logic

Key functions:\
compute_period_returns(...)\
Produces institutional-grade sub-period returns:

Date | Start_Value | External_Flow | End_Value | Period_Return\
compute_twrr(...)\
Geometrically links sub-periods:\

TWRR = Π (1 + r_t) - 1\
This removes cash flow distortion.

# 🧭 sector_engine.py\
Responsible for sector mapping and attribution scaffolding.

Core responsibilities:\
• Assign securities to sectors\
• Build portfolio vs benchmark weights\
• Prepare attribution datasets

Key functions:\
build_sector_weights(...)\
Produces normalized sector exposures.

build_sector_attribution(...)\
Supports both:\
• Brinson-Fachler\
• Brinson-Hood-Beebower

# 📊 attribution_engine.py
Responsible for performance attribution.

Core responsibilities:\
• Allocation effect\
• Selection effect\
• Interaction effect\
• Asset-level attribution

Supports both institutional attribution models.

Outputs multi-level attribution tables.

# ⚠️ risk_engine.py
Responsible for portfolio risk modeling.

Core responsibilities:\
• Volatility modeling\
• Covariance matrices\
• Correlation heatmaps\
• Diversification ratios\
• VaR decomposition\
• Marginal / Component VaR

Typical outputs:\
• Asset return matrix\
• Correlation matrix\
• Portfolio σ\
• MVaR, CVaR\
• Risk attribution tables

# 🌍 exposure_engine.py

Responsible for market and factor exposure diagnostics.

This engine evaluates how portfolio excess returns relate to external drivers, without asserting causality or predictive power.

Core responsibilities

• Construct portfolio excess return series\
• Align market index excess returns\
• Incorporate FX and macro proxies\
• Run OLS-based exposure regressions\
• Compute statistical diagnostics\
• Reconcile regression output with attribution math

Typical factors

• Market index excess return\
• FX return (USD/JMD)\
• Interest rate proxies\
• Optional alternative indices

Outputs

• Beta coefficients\
• Alpha (intercept)\
• R-squared and adjusted R-squared\
• t-stats and p-values\
• Durbin–Watson statistic\
• Condition number\
• Residual diagnostics

This engine answers:

“What risks am I implicitly taking, and does the data support that?”

It is diagnostic, not a factor model factory.

# 🧩 ui_components.py

Responsible for shared UI construction and analytical presentation.

Core responsibilities

• Standardized tables\
• Metric cards\
• Regression summaries\
• Diagnostic panels\
• Attribution and exposure visual scaffolding

This module ensures:\
• consistent presentation\
• separation of UI and financial logic\
• reusability across pages

No financial calculations occur in this layer.

# 🖥️ 5_Market_Exposure.py

Streamlit orchestration layer for exposure diagnostics.

Responsibilities

• Assemble cleaned excess return datasets\
• Invoke exposure_engine\
• Display regression results and diagnostics\
• Present exposure coefficients and attribution reconciliation\
• Surface model integrity warnings

This page intentionally:\
• avoids optimization\
• avoids forecasting\
• avoids black-box factor abstractions

It exists to help users interpret portfolio behavior in market context.

# 🗄️ Reference Data Layer
All market data is:

• Precompiled\
• Pickled\
• AES-encrypted\
• Loaded into memory\
• Destroyed from disk immediately

This enables:

✔ fast startup\
✔ IP protection\
✔ reproducibility\
✔ offline operation

🎯 Design Philosophy
Portfolio Lab is designed around:

• determinism\
• auditability\
• separation of concerns\
• institutional math\
• reproducible analytics

The app avoids “black-box” finance.

Every metric is built from:

Transactions → Positions → Valuations → Returns → Attribution → Risk → Market Exposure

# 🧩 Why this structure matters
This engine-based design allows:

• easy testing\
• research extensions\
• simulation engines\
• benchmark expansion\
• multi-user architecture\
• AI portfolio assistants

🧪 Validation mindset\
Each engine can be unit-tested independently.

If a result is wrong:\
it is always traceable to one layer.

This makes Portfolio Lab suitable for:\
• education\
• research\
• analytics tooling\
• performance reporting\
• emerging-market factor exploration

# 📌 Final note\
Portfolio Lab is not a trading app.\
It is not an optimizer.\
It is not a forecasting engine.\

It is a portfolio intelligence system.

It reconstructs the full economic history of a portfolio and then asks:

“What does the math say about how this portfolio actually behaves?”

That distinction is what allows professional-grade analytics to exist on top of it.
