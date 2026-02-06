````md
# VeighNa — By Traders, For Traders. AI-Powered.

VeighNa is a Python-based, open-source quantitative trading framework that has grown—step by step—into a multi-functional quantitative trading platform through continuous contributions from the community. Since its release, it has attracted many users from financial institutions and related fields, including private equity funds, securities companies, futures companies, and more.

## Getting Help

If you have any questions during secondary development with VeighNa (strategies, modules, etc.):

1. Check the **VeighNa Project Documentation**: https://www.vnpy.com/docs/cn/index.html  
2. If unresolved, visit the **Official Community Forum** and post in **Questions and Help**: https://www.vnpy.com/forum/  
3. You’re also welcome to share learnings in **Experience Sharing**.

## Join the Community

**Want more information about VeighNa?** Scan the QR code below to add the assistant and join the **VeighNa Community Exchange WeChat Group**.

<p align="center">
  <img src="https://vnpy.oss-cn-shanghai.aliyuncs.com/github_wx.png" width="250" alt="WeChat QR code" />
</p>

---

## AI-Powered

On the tenth anniversary of VeighNa’s release, **version 4.0** officially launched with the new [`vnpy.alpha`](./vnpy/alpha) module for AI quantitative strategies—providing professional quantitative traders with a **one-stop multi-factor machine learning (ML) strategy development, research, and live trading solution**.

<p align="center">
  <img src="https://vnpy.oss-cn-shanghai.aliyuncs.com/alpha_demo.jpg" width="500" alt="vnpy.alpha demo" />
</p>

### `vnpy.alpha` Overview

- 📊 **[`dataset`](./vnpy/alpha/dataset)** — Factor Feature Engineering  
  - Optimized for ML algorithm training, supporting efficient batch feature computation and processing  
  - Built-in factor expression engine for one-click training dataset generation  
  - **[Alpha 158](./vnpy/alpha/dataset/datasets/alpha_158.py)**: Stock feature set from Microsoft’s Qlib project, covering candlestick patterns, price trends, time-series volatility, and more  

- 💡 **[`model`](./vnpy/alpha/model)** — Prediction Model Training  
  - Standardized ML templates to simplify model construction and training  
  - Unified API design to switch algorithms and compare performance  
  - Includes multiple mainstream algorithms:  
    - **[Lasso](./vnpy/alpha/model/models/lasso_model.py)**: L1-regularized regression for feature selection  
    - **[LightGBM](./vnpy/alpha/model/models/lgb_model.py)**: Efficient GBDT optimized for large datasets  
    - **[MLP](./vnpy/alpha/model/models/mlp_model.py)**: Multi-layer perceptron for non-linear modeling  

- 🤖 **[`strategy`](./vnpy/alpha/strategy)** — Strategy Research & Development  
  - Build trading strategies based on ML signal prediction models  
  - Supports cross-sectional multi-target and time-series single-target strategy types  

- 🔬 **[`lab`](./vnpy/alpha/lab.py)** — Research Process Management  
  - End-to-end workflow: data management → training → signal generation → backtesting  
  - Clean API with built-in visualization for evaluating strategy performance and model effects  

- 📚 **[`notebook`](./examples/alpha_research)** — Quant Research Demos  
  - **[download_data_rq](./examples/alpha_research/download_data_rq.ipynb)**: Download A-share index constituents via RQData (incl. constituent changes + historical data)  
  - **[download_data_xt](./examples/alpha_research/download_data_xt.ipynb)**: Download A-share constituent changes + candlestick data via XtQuant  
  - **[research_workflow_lasso](./examples/alpha_research/research_workflow_lasso.ipynb)**: Lasso-based workflow (feature selection + prediction)  
  - **[research_workflow_lgb](./examples/alpha_research/research_workflow_lgb.ipynb)**: LightGBM-based workflow (ensemble prediction)  
  - **[research_workflow_mlp](./examples/alpha_research/research_workflow_mlp.ipynb)**: MLP-based workflow (deep learning in quant trading)

The design philosophy of `vnpy.alpha` is inspired by the **Qlib** project: https://github.com/microsoft/qlib  
Special thanks to the Qlib development team!

---

## Functional Features

Modules marked with **:arrow_up:** have completed upgrade compatibility testing for **v4.0**. The 4.0 core framework prioritizes compatibility, so most modules can be used directly (interfaces involving C++ API encapsulation must be upgraded before use).

1. **:arrow_up: Multi-functional trading platform (`trader`)**  
   Integrates multiple trading interfaces and provides simple, easy-to-use APIs for strategy and module development—enabling rapid creation of quantitative trading applications.

2. **Trading interfaces (`gateway`) supported (domestic & overseas)**

   - **Domestic Market**
     - :arrow_up: CTP ([ctp](https://www.github.com/vnpy/vnpy_ctp)) — Domestic futures, options  
     - :arrow_up: CTP Mini ([mini](https://www.github.com/vnpy/vnpy_mini)) — Domestic futures, options  
     - :arrow_up: CTP Securities ([sopt](https://www.github.com/vnpy/vnpy_sopt)) — ETF options  
     - :arrow_up: FEMAS ([femas](https://www.github.com/vnpy/vnpy_femas)) — Domestic futures  
     - :arrow_up: Hengsheng UFT ([uft](https://www.github.com/vnpy/vnpy_uft)) — Domestic futures, ETF options  
     - :arrow_up: Esunny ([esunny](https://www.github.com/vnpy/vnpy_esunny)) — Domestic futures, Gold TD  
     - :arrow_up: Apex HTS ([hts](https://www.github.com/vnpy/vnpy_hts)) — ETF options  
     - :arrow_up: Apex Feichuang ([sec](https://www.github.com/vnpy/vnpy_sec)) — ETF options  
     - :arrow_up: Zhongtai XTP ([xtp](https://www.github.com/vnpy/vnpy_xtp)) — A-shares, ETF options  
     - :arrow_up: Huaxin Qidian ([tora](https://www.github.com/vnpy/vnpy_tora)) — A-shares, ETF options  
     - Dongzheng OST ([ost](https://www.github.com/vnpy/vnpy_ost)) — A-shares  
     - Dongfang Caifu EMT ([emt](https://www.github.com/vnpy/vnpy_emt)) — A-shares  
     - Feishu ([sgit](https://www.github.com/vnpy/vnpy_sgit)) — Gold TD, domestic futures  
     - :arrow_up: Jinshida Gold ([ksgold](https://www.github.com/vnpy/vnpy_ksgold)) — Gold TD  
     - :arrow_up: Lixing Asset Management ([lstar](https://www.github.com/vnpy/vnpy_lstar)) — Futures asset management  
     - :arrow_up: Ronghang ([rohon](https://www.github.com/vnpy/vnpy_rohon)) — Futures asset management  
     - :arrow_up: Jieyisi ([jees](https://www.github.com/vnpy/vnpy_jees)) — Futures asset management  
     - Zhonghui Yida ([comstar](https://www.github.com/vnpy/vnpy_comstar)) — Interbank market  
     - :arrow_up: TTS ([tts](https://www.github.com/vnpy/vnpy_tts)) — Domestic futures (simulation)  

   - **Overseas Market**
     - :arrow_up: Interactive Brokers ([ib](https://www.github.com/vnpy/vnpy_ib)) — Overseas securities, futures, options, precious metals  
     - :arrow_up: Esunny 9.0 Overseas ([tap](https://www.github.com/vnpy/vnpy_tap)) — Overseas futures  
     - :arrow_up: Zhida Futures ([da](https://www.github.com/vnpy/vnpy_da)) — Overseas futures  

   - **Special Applications**
     - :arrow_up: RQData Market Data ([rqdata](https://www.github.com/vnpy/vnpy_rqdata)) — Cross-market real-time data  
     - :arrow_up: XtQuant Market Data ([xt](https://www.github.com/vnpy/vnpy_xt)) — Cross-market real-time data  
     - :arrow_up: RPC Service ([rpc](https://www.github.com/vnpy/vnpy_rpcservice)) — IPC interface for distributed architecture  

3. **Strategy applications (`app`)**
   - :arrow_up: [cta_strategy](https://www.github.com/vnpy/vnpy_ctastrategy) — CTA engine, fine-grained order/cancel control  
   - :arrow_up: [cta_backtester](https://www.github.com/vnpy/vnpy_ctabacktester) — GUI backtesting & parameter optimization (no notebook required)  
   - :arrow_up: [spread_trading](https://www.github.com/vnpy/vnpy_spreadtrading) — Custom spreads, quotes/positions, algo & auto modes  
   - :arrow_up: [option_master](https://www.github.com/vnpy/vnpy_optionmaster) — Options models, IV surface, Greeks & risk tracking  
   - :arrow_up: [portfolio_strategy](https://www.github.com/vnpy/vnpy_portfoliostrategy) — Multi-contract strategies w/ backtest + live trading  
   - :arrow_up: [algo_trading](https://www.github.com/vnpy/vnpy_algotrading) — TWAP, Sniper, Iceberg, BestLimit, etc.  
   - :arrow_up: [script_trader](https://www.github.com/vnpy/vnpy_scripttrader) — Script strategies + REPL trading (no backtesting)  
   - :arrow_up: [paper_account](https://www.github.com/vnpy/vnpy_paperaccount) — Local paper trading & matching using real-time data  
   - :arrow_up: [chart_wizard](https://www.github.com/vnpy/vnpy_chartwizard) — Candlestick charts w/ historical + live ticks  
   - :arrow_up: [portfolio_manager](https://www.github.com/vnpy/vnpy_portfoliomanager) — Sub-accounts, execution logs, PnL stats  
   - :arrow_up: [rpc_service](https://www.github.com/vnpy/vnpy_rpcservice) — Server/client routing for multi-process systems  
   - :arrow_up: [data_manager](https://www.github.com/vnpy/vnpy_datamanager) — View/import/export historical data (CSV supported)  
   - :arrow_up: [data_recorder](https://www.github.com/vnpy/vnpy_datarecorder) — Record ticks/bars to DB in real time  
   - :arrow_up: [excel_rtd](https://www.github.com/vnpy/vnpy_excelrtd) — Excel RTD streaming via pyxll  
   - :arrow_up: [risk_manager](https://www.github.com/vnpy/vnpy_riskmanager) — Flow control, order limits, cancel limits, etc.  
   - :arrow_up: [web_trader](https://www.github.com/vnpy/vnpy_webtrader) — REST + WebSocket server for B/S architecture  

4. **Trading API encapsulation (`api`)**
   - :arrow_up: REST Client ([rest](https://www.github.com/vnpy/vnpy_rest)) — Coroutine-based async IO for high concurrency  
   - :arrow_up: WebSocket Client ([websocket](https://www.github.com/vnpy/vnpy_websocket)) — Async client sharing event loop with REST  

5. **:arrow_up: Event-driven engine (`event`)**  
   Core engine for event-driven trading programs.

6. **Database adapters (`database`)**
   - **SQL**
     - :arrow_up: SQLite ([sqlite](https://www.github.com/vnpy/vnpy_sqlite)) — Lightweight single-file DB (default, beginner-friendly)  
     - :arrow_up: MySQL ([mysql](https://www.github.com/vnpy/vnpy_mysql)) — Mainstream relational DB, rich docs, NewSQL-friendly  
     - :arrow_up: PostgreSQL ([postgresql](https://www.github.com/vnpy/vnpy_postgresql)) — Feature-rich (recommended for experienced users)  
   - **NoSQL**
     - DolphinDB ([dolphindb](https://www.github.com/vnpy/vnpy_dolphindb)) — High-performance distributed time-series DB  
     - :arrow_up: TDengine ([taos](https://www.github.com/vnpy/vnpy_taos)) — Distributed time-series DB w/ caching & streaming  
     - :arrow_up: MongoDB ([mongodb](https://www.github.com/vnpy/vnpy_mongodb)) — Document DB w/ hot data memory cache  

7. **Data service adapters (`datafeed`)**
   - :arrow_up: XtQuant ([xt](https://www.github.com/vnpy/vnpy_xt)) — Stocks, futures, options, funds, bonds  
   - :arrow_up: RiceQuant RQData ([rqdata](https://www.github.com/vnpy/vnpy_rqdata)) — Stocks, futures, options, funds, bonds, Gold TD  
   - :arrow_up: MultiCharts ([mcdata](https://www.github.com/vnpy/vnpy_mcdata)) — Futures, futures options  
   - :arrow_up: TuShare ([tushare](https://www.github.com/vnpy/vnpy_tushare)) — Stocks, futures, options, funds  
   - :arrow_up: Wind ([wind](https://www.github.com/vnpy/vnpy_wind)) — Stocks, futures, funds, bonds  
   - :arrow_up: THS iFinD ([ifind](https://www.github.com/vnpy/vnpy_ifind)) — Stocks, futures, funds, bonds  
   - :arrow_up: TianQin TQSDK ([tqsdk](https://www.github.com/vnpy/vnpy_tqsdk)) — Futures  
   - :arrow_up: GoldMiner ([gm](https://www.github.com/vnpy/vnpy_gm)) — Stocks  
   - :arrow_up: Polygon ([polygon](https://www.github.com/vnpy/vnpy_polygon)) — Stocks, futures, options  

8. **:arrow_up: Cross-process communication (`rpc`)**  
   Standard component for distributed deployments.

9. **:arrow_up: High-performance charting (`chart`)**  
   Large-scale candlestick visualization with real-time updates.

10. Community resources:
   - Forum: http://www.vnpy.com/forum  
   - Zhihu Column: http://zhuanlan.zhihu.com/vn-py  

11. Official QQ exchange group **262656087** (strictly managed; entry fee donated to the VeighNa community fund).

> **Note:** Feature lists reflect the state at the time of documentation release and may change later. If there are discrepancies, please open an Issue so the docs can be updated.

---

## Environment Preparation

- Recommended: **VeighNa Studio 4.3.0** (bundled distribution with VeighNa framework + VeighNa Station):  
  https://download.vnpy.com/veighna_studio-4.3.0.exe
- Supported OS:
  - Windows 11+ / Windows Server 2022+
  - Ubuntu 22.04 LTS+
- Supported Python: **3.10+ (64-bit)** — **Recommended: Python 3.13**

## Installation Steps

Download the Release version here: https://github.com/vnpy/vnpy/releases  
Unzip and run:

### Windows
```bat
install.bat
````

### Ubuntu

```bash
bash install.sh
```

### macOS

```bash
bash install_osx.sh
```

---

## Usage Guide

1. Register a CTP simulation account at [http://www.simnow.com.cn/](http://www.simnow.com.cn/)
2. Register at the VeighNa Community Forum to obtain your VeighNa Station account password (same as forum password): [https://www.vnpy.com/forum/](https://www.vnpy.com/forum/)
3. Launch **VeighNa Station** (installed with VeighNa Studio) and log in
4. Click **VeighNa Trader** at the bottom to start trading

**Note:** Do not close VeighNa Station while VeighNa Trader is running (it will exit automatically).

---

## Script Running

Besides launching via VeighNa Station, you can create a `run.py` file anywhere and use:

```python
from vnpy.event import EventEngine
from vnpy.trader.engine import MainEngine
from vnpy.trader.ui import MainWindow, create_qapp
from vnpy_ctp import CtpGateway
from vnpy_ctastrategy import CtaStrategyApp
from vnpy_ctabacktester import CtaBacktesterApp


def main():
    """Start VeighNa Trader"""
    qapp = create_qapp()
    event_engine = EventEngine()
    main_engine = MainEngine(event_engine)

    main_engine.add_gateway(CtpGateway)
    main_engine.add_app(CtaStrategyApp)
    main_engine.add_app(CtaBacktesterApp)

    main_window = MainWindow(main_engine, event_engine)
    main_window.showMaximized()
    qapp.exec()


if __name__ == "__main__":
    main()
```

Then in that directory (CMD/PowerShell), run:

```bash
python run.py
```

---

## Contributing Code

VeighNa is hosted on GitHub. To contribute, use the PR (Pull Request) process:

1. Create an Issue (recommended for larger changes): [https://github.com/vnpy/vnpy/issues/new](https://github.com/vnpy/vnpy/issues/new)
2. Fork the repo: [https://github.com/vnpy/vnpy](https://github.com/vnpy/vnpy) (click **Fork**)
3. Clone your fork

   * If your fork is outdated, sync it: [https://help.github.com/articles/syncing-a-fork/](https://help.github.com/articles/syncing-a-fork/)
4. Create a feature branch from **dev**:

```bash
git checkout -b $my_feature_branch dev
```

5. Make changes and push to your fork
6. Open a Pull Request to the upstream **dev** branch:

   * Use **compare across forks**: [https://github.com/vnpy/vnpy/compare?expand=1](https://github.com/vnpy/vnpy/compare?expand=1)
7. Iterate on review feedback until merged

### Code Quality Rules

* Use **ruff** for style checks:

```bash
ruff check .
```

* Use **mypy** for static type checks:

```bash
mypy vnpy
```

---

## Other Content

* Support: [https://github.com/vnpy/vnpy/blob/dev/.github/SUPPORT.md](https://github.com/vnpy/vnpy/blob/dev/.github/SUPPORT.md)
* Code of Conduct: [https://github.com/vnpy/vnpy/blob/dev/.github/CODE_OF_CONDUCT.md](https://github.com/vnpy/vnpy/blob/dev/.github/CODE_OF_CONDUCT.md)
* Issue Template: [https://github.com/vnpy/vnpy/blob/dev/.github/ISSUE_TEMPLATE.md](https://github.com/vnpy/vnpy/blob/dev/.github/ISSUE_TEMPLATE.md)
* PR Template: [https://github.com/vnpy/vnpy/blob/dev/.github/PULL_REQUEST_TEMPLATE.md](https://github.com/vnpy/vnpy/blob/dev/.github/PULL_REQUEST_TEMPLATE.md)

---

## License

MIT

```
```
