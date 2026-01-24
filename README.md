# 📊About Portfolio-Lab — Portfolio Analytics Dashboard
![Portfolio Lab Demo](https://github.com/user-attachments/assets/8ab8eae8-ca18-412f-9960-232f3ce1aa82)
## Overview

**Portfolio-Lab** is a web-based portfolio analytics application built with **Streamlit**.  

It is designed to help investors reconstruct, evaluate, and understand their portfolios through a professional-grade performance and risk analytics pipeline.


This version of the platform focuses on **Jamaica Stock Exchange (JSE)** listed equities.  

Users manually input transactions, and the system rebuilds holdings, valuations, performance, attribution, and risk metrics from first principles.


At this stage, Portfolio-Lab does **not persist user data** and is intended as an **analytics laboratory** rather than a brokerage or trading platform.


---

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://portfolio-analytics-lab.streamlit.app)

## Purpose


Portfolio-Lab was built to explore how institutional-grade portfolio analytics can be made transparent, interpretable, and accessible.


Rather than emphasizing trading, the app is centered on:

- understanding portfolio construction  
- measuring performance correctly  
- identifying sources of return (alpha)  
- evaluating portfolio risk and diversification  
- building financial intuition through structure  

This project treats portfolios not as tickers, but as evolving financial systems.


---


## Core Capabilities


### 🔁 Portfolio Reconstruction Engine

- Transaction-based holdings reconstruction  
- Corporate-action aware quantity roll-forward  
- Cost basis tracking  
- Multi-currency normalization into JMD  
- Historical valuation snapshots  

---

### 📈 Performance Engine

- Simple returns  
- Time-Weighted Rate of Return (TWRR)  
- Money-Weighted Rate of Return (MWRR / IRR)  
- CFA-aligned TWRR logic (non-annualized sub-year periods)  
- Portfolio valuation history builder  

---


### 🧮 Attribution Engine

- Asset-level attribution  
- Sector-level attribution  
- Brinson-Hood-Beebower (BHB) model  
- Brinson-Fachler (BF) model  
- Allocation, selection, and interaction effects  

Designed to evaluate **where** returns came from — not only how much.


---


### ⚖️ Risk Lab

- Volatility analysis  
- Correlation heatmap  
- Diversification ratio  
- Sharpe ratio  
- Value-at-Risk (95% and 99%)  
- Marginal, incremental, and component VaR  
- Asset and sector risk attribution  

Built to explore **how portfolio structure influences risk**.

---


## Architectural Design


Portfolio-Lab is structured as a set of independent but connected financial engines:
- valuation_engine.py → portfolio valuation logic
- holdings_engine.py → transaction → holdings reconstruction
- returns_engine.py → TWRR / MWRR / performance metrics
- sector_engine.py → sector aggregation & attribution
- attribution_engine.py → BF / BHB performance attribution
- risk_engine.py → risk & VaR analytics

The Streamlit app orchestrates these engines into interactive workflows:
1. 1_Transactions.py
2. 2_Holdings.py
3. 3_Performance_Attribution.py
4. 4_Risk_Attribution.py

This modular structure allows each financial problem domain to remain isolated, testable, and extensible.

---

## Distinctiveness

Portfolio-Lab differs from typical “portfolio trackers” in that it is built as a **multi-engine financial infrastructure** rather than a dashboard wrapper.

It reconstructs portfolios from raw transactions and evaluates them across:

- valuation  
- performance  
- attribution  
- and risk  

using academically grounded and industry-standard models.

The emphasis is not on visualizing prices, but on **explaining portfolio behavior.**

---

## Complexity Statement

This project addresses several technically and conceptually complex domains:

- transaction-driven portfolio reconstruction  
- cash-flow-neutral performance measurement  
- benchmark-relative attribution modeling  
- historical valuation pipelines  
- and decomposed portfolio risk  

Each engine required aligning financial theory, accounting identities, and software design into a consistent system.

Portfolio-Lab represents an applied attempt to translate institutional portfolio analytics into an interpretable, user-facing platform.

---

## Project Structure
```bash
portfolio-analytics-dashboard/
│
├── app.py
├── pages/
│ ├── 1_Transactions.py
│ ├── 2_Holdings.py
│ ├── 3_Performance_Attribution.py
│ └── 4_Risk_Attribution.py
│
├── attribution_engine.py
├── holdings_engine.py
├── returns_engine.py
├── risk_engine.py
├── sector_engine.py
├── valuation_engine.py
└── simulation_engine.py
```

---

## Installation

1. Clone the repository  
2. Install dependencies  
3. Run the Streamlit app  

```bash
streamlit run app.py
```

Then open the provided local URL in your browser.

---

## Current Limitations

- No user authentication
- No persistent database
- Manual transaction input only
- JSE equity focus
- Not intended for live trading or investment advice
- Portfolio-Lab is currently an analytics sandbox and research platform.

## Out-of-Scope (Planned)

* Portfolio simulation engine
* Optimization framework
* Automated data ingestion
* User profiles and storage
* Scenario and stress-testing modules
* Development Philosophy

This project is not positioned as a finished financial product.

## 🛡️ Security & Privacy
Reference data is stored in an AES-256 encrypted volume and decrypted in-memory during session initialization to ensure data integrity. 
We take data privacy seriously. **Portfolio-Lab does not persist user data.** * All uploaded transactions are processed in-memory and destroyed when the session ends.
* Technical details on our encryption and data handling can be found in our [Security & Privacy Documentation](docs/security.md).

**It is an evolving collaboration between human design, financial theory, and artificial intelligence — built to explore how portfolio intelligence tools can be constructed, validated, and eventually democratized.**

Portfolio-Lab is an experiment in learning, architecture, and applied finance.

## Additional Documentation

📘 [User Guide](\docs\howtouse.md)  
🧠 [Technical Docs](\docs\documentation.md)

## 🤝 Get Involved

* 🙋‍♂️ **Have a question?** Join our [GitHub Discussions](https://github.com/Dame-Sky/Portfolio-Analytics-Lab/discussions)
* 💡 **Have an idea?** Submit a [Feature Request](https://github.com/Dame-Sky/Portfolio-Analytics-Lab/issues/new?template=feature_request.md)
* 🗺️ **See what's coming next:** View our [Project Roadmap](https://github.com/users/Dame-Sky/projects/1)
