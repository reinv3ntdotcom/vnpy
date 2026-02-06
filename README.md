# VeighNa - By Traders, For Traders, AI-Powered.
<p align="center">
  <img src ="https://vnpy.oss-cn-shanghai.aliyuncs.com/veighna-logo.png"/>
</p>
💬 Want to read this in **chinese** ? Go [**here**](README.md)
<p align="center">
    <img src ="https://img.shields.io/badge/version-4.3.0-blueviolet.svg"/>
    <img src ="https://img.shields.io/badge/platform-windows|linux|macos-yellow.svg"/>
    <img src ="https://img.shields.io/badge/python-3.10|3.11|3.12|3.13-blue.svg" />
    <img src ="https://img.shields.io/github/actions/workflow/status/vnpy/vnpy/pythonapp.yml?branch=master"/>
    <img src ="https://img.shields.io/github/license/vnpy/vnpy.svg?color=orange"/>
</p>
VeighNa is a Python-based open source quantitative trading system development framework that has grown step by step into a multi-functional quantitative trading platform with continuous contributions from the open source community. Since its release, it has accumulated numerous users from financial institutions or related fields, including private equity funds, securities companies, futures companies, etc.

If you have any questions during secondary development using VeighNa (strategies, modules, etc.), please check the [**VeighNa Project Documentation**](https://www.vnpy.com/docs/cn/index.html). If unresolved, please go to the [Questions and Help] section of the [**Official Community Forum**](https://www.vnpy.com/forum/) for assistance. You are also welcome to share your experiences in the [Experience Sharing] section!

**Want to get more information about VeighNa?** Please scan the QR code below to add the assistant and join the [VeighNa Community Exchange WeChat Group]:
<p align="center">
  <img src ="https://vnpy.oss-cn-shanghai.aliyuncs.com/github_wx.png"/, width=250>
</p>
## AI-Powered
On the tenth anniversary of VeighNa's release, version 4.0 was officially launched, featuring the new [vnpy.alpha](./vnpy/alpha) module for AI quantitative strategies, providing professional quantitative traders with **a one-stop multi-factor machine learning (ML) strategy development, research, and live trading solution**:
<p align="center">
  <img src ="https://vnpy.oss-cn-shanghai.aliyuncs.com/alpha_demo.jpg"/, width=500>
</p>
* :bar_chart: **[dataset](./vnpy/alpha/dataset)**: Factor Feature Engineering
    * Optimized for ML algorithm training, supporting efficient batch feature computation and processing
    * Built-in rich factor feature expression calculation engine for quick one-click generation of training data
    * [Alpha 158](./vnpy/alpha/dataset/datasets/alpha_158.py): Stock market feature set from Microsoft's Qlib project, covering candlestick patterns, price trends, time-series volatility, and other multi-dimensional quantitative factors
* :bulb: **[model](./vnpy/alpha/model)**: Prediction Model Training
    * Provides standardized ML model development templates, greatly simplifying model construction and training processes
    * Unified API interface design, supporting seamless switching between different algorithms for performance comparison testing
    * Integrates multiple mainstream machine learning algorithms:
        * [Lasso](./vnpy/alpha/model/models/lasso_model.py): Classic Lasso regression model, achieving feature selection through L1 regularization
        * [LightGBM](./vnpy/alpha/model/models/lgb_model.py): Efficient gradient boosting decision tree, with training engine optimized for large-scale datasets
        * [MLP](./vnpy/alpha/model/models/mlp_model.py): Multi-layer perceptron neural network, suitable for complex non-linear relationship modeling
* :robot: **[strategy](./vnpy/alpha/strategy)**: Strategy Research and Development
    * Quickly build quantitative trading strategies based on ML signal prediction models
    * Supports cross-sectional multi-target and time-series single-target strategy types
* :microscope: **[lab](./vnpy/alpha/lab.py)**: Research Process Management
    * Integrates complete workflows for data management, model training, signal generation, and strategy backtesting
    * Concise API design with built-in visualization analysis tools for intuitive evaluation of strategy performance and model effects
* :book: **[notebook](./examples/alpha_research)**: Quantitative Research Demo
    * [download_data_rq](./examples/alpha_research/download_data_rq.ipynb): Download A-share index constituent stock data based on RQData, including index constituent changes tracking and historical market data acquisition
    * [download_data_xt](./examples/alpha_research/download_data_xt.ipynb): Download A-share index constituent historical changes and stock candlestick data based on XtQuant data service
    * [research_workflow_lasso](./examples/alpha_research/research_workflow_lasso.ipynb): Quantitative research workflow based on Lasso regression model, demonstrating linear model feature selection and prediction capabilities
    * [research_workflow_lgb](./examples/alpha_research/research_workflow_lgb.ipynb): Quantitative research workflow based on LightGBM gradient boosting tree, using efficient ensemble learning methods for prediction
    * [research_workflow_mlp](./examples/alpha_research/research_workflow_mlp.ipynb): Quantitative research workflow based on multi-layer perceptron neural network, demonstrating the application of deep learning in quantitative trading
The design philosophy of the vnpy.alpha module is inspired by the [Qlib](https://github.com/microsoft/qlib) project, providing powerful AI quantitative capabilities while maintaining ease of use. Special thanks to the Qlib development team!
## Functional Features
Modules marked with :arrow_up: represent those that have completed upgrade compatibility testing for version 4.0. The 4.0 core framework adopts an upgrade method that prioritizes compatibility, so most modules can be used directly (interfaces involving C++ API encapsulation must be upgraded before use).
1. :arrow_up: Multi-functional quantitative trading platform (trader), integrating multiple trading interfaces and providing simple and easy-to-use APIs for specific strategy algorithms and functional development, for quickly building quantitative trading applications needed by traders.
2. Covering trading interfaces (gateway) for the following trading varieties domestically and internationally:
    * Domestic Market
        * :arrow_up: CTP ([ctp](https://www.github.com/vnpy/vnpy_ctp)): Domestic futures, options
        * :arrow_up: CTP Mini ([mini](https://www.github.com/vnpy/vnpy_mini)): Domestic futures, options
        * :arrow_up: CTP Securities ([sopt](https://www.github.com/vnpy/vnpy_sopt)): ETF options
        * :arrow_up: FEMAS ([femas](https://www.github.com/vnpy/vnpy_femas)): Domestic futures
        * :arrow_up: Hengsheng UFT ([uft](https://www.github.com/vnpy/vnpy_uft)): Domestic futures, ETF options
        * :arrow_up: Esunny ([esunny](https://www.github.com/vnpy/vnpy_esunny)): Domestic futures, Gold TD
        * :arrow_up: Apex HTS ([hts](https://www.github.com/vnpy/vnpy_hts)): ETF options
        * :arrow_up: Apex Feichuang ([sec](https://www.github.com/vnpy/vnpy_sec)): ETF options
        * :arrow_up: Zhongtai XTP ([xtp](https://www.github.com/vnpy/vnpy_xtp)): Domestic securities (A-shares), ETF options
        * :arrow_up: Huaxin Qidian ([tora](https://www.github.com/vnpy/vnpy_tora)): Domestic securities (A-shares), ETF options
        * Dongzheng OST ([ost](https://www.github.com/vnpy/vnpy_ost)): Domestic securities (A-shares)
        * Dongfang Caifu EMT ([emt](https://www.github.com/vnpy/vnpy_emt)): Domestic securities (A-shares)
        * Feishu ([sgit](https://www.github.com/vnpy/vnpy_sgit)): Gold TD, domestic futures
        * :arrow_up: Jinshida Gold ([ksgold](https://www.github.com/vnpy/vnpy_ksgold)): Gold TD
        * :arrow_up: Lixing Asset Management ([lstar](https://www.github.com/vnpy/vnpy_lstar)): Futures asset management
        * :arrow_up: Ronghang ([rohon](https://www.github.com/vnpy/vnpy_rohon)): Futures asset management
        * :arrow_up: Jieyisi ([jees](https://www.github.com/vnpy/vnpy_jees)): Futures asset management
        * Zhonghui Yida ([comstar](https://www.github.com/vnpy/vnpy_comstar)): Interbank market
        * :arrow_up: TTS ([tts](https://www.github.com/vnpy/vnpy_tts)): Domestic futures (simulation)
    * Overseas Market
        * :arrow_up: Interactive Brokers ([ib](https://www.github.com/vnpy/vnpy_ib)): Overseas securities, futures, options, precious metals, etc.
        * :arrow_up: Esunny 9.0 Overseas ([tap](https://www.github.com/vnpy/vnpy_tap)): Overseas futures
        * :arrow_up: Zhida Futures ([da](https://www.github.com/vnpy/vnpy_da)): Overseas futures
    * Special Applications
        * :arrow_up: RQData Market Data ([rqdata](https://www.github.com/vnpy/vnpy_rqdata)): Cross-market (stocks, indices, ETFs, futures) real-time market data
        * :arrow_up: XtQuant Market Data ([xt](https://www.github.com/vnpy/vnpy_xt)): Cross-market (stocks, indices, convertible bonds, ETFs, futures, options) real-time market data
        * :arrow_up: RPC Service ([rpc](https://www.github.com/vnpy/vnpy_rpcservice)): Inter-process communication interface for distributed architecture
3. Covering the following types of quantitative strategy trading applications (app):
    * :arrow_up: [cta_strategy](https://www.github.com/vnpy/vnpy_ctastrategy): CTA strategy engine module, allowing fine-grained control over order placement and cancellation behavior during CTA strategy operation while maintaining ease of use (reducing trading slippage, implementing high-frequency strategies)
    * :arrow_up: [cta_backtester](https://www.github.com/vnpy/vnpy_ctabacktester): CTA strategy backtesting module, no need for Jupyter Notebook, directly use graphical interface for strategy backtesting analysis, parameter optimization, etc.
    * :arrow_up: [spread_trading](https://www.github.com/vnpy/vnpy_spreadtrading): Spread trading module, supports custom spreads, real-time calculation of spread quotes and positions, supports spread algorithm trading and automatic spread strategy modes
    * :arrow_up: [option_master](https://www.github.com/vnpy/vnpy_optionmaster): Options trading module, designed for the domestic options market, supports multiple option pricing models, implied volatility surface calculation, Greek values risk tracking, etc.
    * :arrow_up: [portfolio_strategy](https://www.github.com/vnpy/vnpy_portfoliostrategy): Portfolio strategy module, for quantitative strategies trading multiple contracts simultaneously (Alpha, option arbitrage, etc.), providing historical data backtesting and live automatic trading functions
    * :arrow_up: [algo_trading](https://www.github.com/vnpy/vnpy_algotrading): Algorithm trading module, providing various common intelligent trading algorithms: TWAP, Sniper, Iceberg, BestLimit, etc.
    * :arrow_up: [script_trader](https://www.github.com/vnpy/vnpy_scripttrader): Script strategy module, designed for multi-asset quantitative strategies and computational tasks, can also implement REPL command-form trading in the command line, does not support backtesting
    * :arrow_up: [paper_account](https://www.github.com/vnpy/vnpy_paperaccount): Local simulation module, pure local implementation of simulated trading functions, based on real-time market data from trading interfaces for order matching, providing order execution pushes and position records
    * :arrow_up: [chart_wizard](https://www.github.com/vnpy/vnpy_chartwizard): Candlestick chart module, obtains historical data based on RQData data service (futures) or trading interfaces, and displays real-time market changes combined with Tick pushes
    * :arrow_up: [portfolio_manager](https://www.github.com/vnpy/vnpy_portfoliomanager): Trading portfolio management module, based on independent strategy trading portfolios (sub-accounts), providing order execution record management, automatic trading position tracking, and real-time daily profit/loss statistics
    * :arrow_up: [rpc_service](https://www.github.com/vnpy/vnpy_rpcservice): RPC service module, allows starting a process as a server, as a unified market data and trading routing channel, allowing multiple clients to connect simultaneously, implementing multi-process distributed systems
    * :arrow_up: [data_manager](https://www.github.com/vnpy/vnpy_datamanager): Historical data management module, view database data overview through tree directory, select any time period data to view field details, supports CSV file data import and export
    * :arrow_up: [data_recorder](https://www.github.com/vnpy/vnpy_datarecorder): Market data recording module, configured via graphical interface, records Tick or candlestick data to database in real-time as needed, for strategy backtesting or live initialization
    * :arrow_up: [excel_rtd](https://www.github.com/vnpy/vnpy_excelrtd): Excel RTD (Real Time Data) real-time data service, implemented based on pyxll module for real-time push updates of various data (market data, contracts, positions, etc.) in Excel
    * :arrow_up: [risk_manager](https://www.github.com/vnpy/vnpy_riskmanager): Risk management module, provides statistics and restrictions on rules including trading flow control, order quantity, active orders, total cancellations, etc., effectively implementing front-end risk control
    * :arrow_up: [web_trader](https://www.github.com/vnpy/vnpy_webtrader): Web service module, designed for B-S architecture needs, implementing a Web server providing active function calls (REST) and passive data pushes (Websocket)
4. Python trading API interface encapsulation (api), providing underlying docking implementation for the above trading interfaces.
    * :arrow_up: REST Client ([rest](https://www.github.com/vnpy/vnpy_rest)): High-performance REST API client based on coroutine asynchronous IO, using event message loop programming model, supporting high-concurrency real-time trading request sending
    * :arrow_up: Websocket Client ([websocket](https://www.github.com/vnpy/vnpy_websocket)): High-performance Websocket API client based on coroutine asynchronous IO, supporting concurrent operation sharing event loop with REST Client
5. :arrow_up: Simple and easy-to-use event-driven engine (event), as the core of event-driven trading programs.
6. Adapter interfaces for various databases (database):
    * SQL Type
        * :arrow_up: SQLite ([sqlite](https://www.github.com/vnpy/vnpy_sqlite)): Lightweight single-file database, no need to install and configure data service programs, VeighNa's default option, suitable for beginner users
        * :arrow_up: MySQL ([mysql](https://www.github.com/vnpy/vnpy_mysql)): Mainstream open-source relational database, extremely rich documentation, and can replace other NewSQL compatible implementations (such as TiDB)
        * :arrow_up: PostgreSQL ([postgresql](https://www.github.com/vnpy/vnpy_postgresql)): Feature-rich open-source relational database, supports adding functions via extension plugins, recommended only for experienced users
    * NoSQL Type
        * DolphinDB ([dolphindb](https://www.github.com/vnpy/vnpy_dolphindb)): High-performance distributed time-series database, suitable for low-latency or real-time tasks with extremely high speed requirements
        * :arrow_up: TDengine ([taos](https://www.github.com/vnpy/vnpy_taos)): Distributed, high-performance, SQL-supporting time-series database, with built-in caching, streaming computation, data subscription, and other system functions, greatly reducing development and operations complexity
        * :arrow_up: MongoDB ([mongodb](https://www.github.com/vnpy/vnpy_mongodb)): Document-oriented database based on distributed file storage (bson format), built-in hot data memory cache provides faster read/write speeds
7. Adapter interfaces for the following data services (datafeed):
    * :arrow_up: XtQuant ([xt](https://www.github.com/vnpy/vnpy_xt)): Stocks, futures, options, funds, bonds
    * :arrow_up: RiceQuant RQData ([rqdata](https://www.github.com/vnpy/vnpy_rqdata)): Stocks, futures, options, funds, bonds, Gold TD
    * :arrow_up: MultiCharts ([mcdata](https://www.github.com/vnpy/vnpy_mcdata)): Futures, futures options
    * :arrow_up: TuShare ([tushare](https://www.github.com/vnpy/vnpy_tushare)): Stocks, futures, options, funds
    * :arrow_up: Wind ([wind](https://www.github.com/vnpy/vnpy_wind)): Stocks, futures, funds, bonds
    * :arrow_up: THS iFinD ([ifind](https://www.github.com/vnpy/vnpy_ifind)): Stocks, futures, funds, bonds
    * :arrow_up: TianQin TQSDK ([tqsdk](https://www.github.com/vnpy/vnpy_tqsdk)): Futures
    * :arrow_up: GoldMiner ([gm](https://www.github.com/vnpy/vnpy_gm)): Stocks
    * :arrow_up: Polygon ([polygon](https://www.github.com/vnpy/vnpy_polygon)): Stocks, futures, options
8. :arrow_up: Cross-process communication standard component (rpc), for implementing complex trading systems with distributed deployment.
9. :arrow_up: Python high-performance candlestick chart (chart), supporting large data volume chart display and real-time data update functions.
10. [Community Forum](http://www.vnpy.com/forum) and [Zhihu Column](http://zhuanlan.zhihu.com/vn-py), content including VeighNa project development tutorials and research on Python applications in quantitative trading.
11. Official exchange group 262656087 (QQ), strict management (regularly remove long-term inactive members), entry fee will be donated to the VeighNa community fund.
Note: The above functional features are listed based on the situation at the time of the documentation release, and there may be subsequent updates or adjustments. If there are discrepancies between the functional description and reality, welcome to contact via Issue for adjustments.
## Environment Preparation
* Recommended to use the Python distribution [VeighNa Studio-4.3.0](https://download.vnpy.com/veighna_studio-4.3.0.exe) specially created by the VeighNa team for quantitative trading, integrated with the built-in VeighNa framework and VeighNa Station quantitative management platform, no manual installation required
* Supported system versions: Windows 11 or above / Windows Server 2022 or above / Ubuntu 22.04 LTS or above
* Supported Python versions: Python 3.10 or above (64-bit), **recommended to use Python 3.13**
## Installation Steps
Download the Release version [here](https://github.com/vnpy/vnpy/releases), unzip and run the following commands to install:
**Windows**
```
install.bat
```
**Ubuntu**
```
bash install.sh
```
**Macos**
```
bash install_osx.sh
```
## Usage Guide
1. Register a CTP simulation account at [SimNow](http://www.simnow.com.cn/), and obtain the broker code and trading market server address on [this page](http://www.simnow.com.cn/product.action).
2. Register at [VeighNa Community Forum](https://www.vnpy.com/forum/) to obtain VeighNa Station account password (same as forum account password)
3. Launch VeighNa Station (after installing VeighNa Studio, a desktop shortcut will be created automatically), enter the account password from the previous step to log in
4. Click the **VeighNa Trader** button at the bottom to start your trading!!!
Note:
* Do not close VeighNa Station during the operation of VeighNa Trader (it will exit automatically)
## Script Running
In addition to the graphical launch method based on VeighNa Station, you can also create run.py in any directory and write the following example code:
```Python
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
Open CMD in that directory (hold Shift -> right-click -> open command window/PowerShell here) and run the following command to start VeighNa Trader:
    python run.py
## Contributing Code
VeighNa uses GitHub to host its source code. If you wish to contribute code, please use GitHub's PR (Pull Request) process:
1. [Create Issue](https://github.com/vnpy/vnpy/issues/new) - For larger changes (such as new features, major refactoring, etc.), it is recommended to open an issue for discussion first. Smaller improvements (such as documentation improvements, bug fixes, etc.) can be submitted directly as PR
2. Fork [VeighNa](https://github.com/vnpy/vnpy) - Click the **Fork** button in the upper right corner
3. Clone your own fork: ```git:disable-run
* If your fork is outdated, manually sync: [Sync method](https://help.github.com/articles/syncing-a-fork/)
4. Create your own feature branch from **dev**: ```git checkout -b $my_feature_branch dev```
5. Make changes on $my_feature_branch and push the changes to your fork
6. Create a [Pull Request] from your fork's $my_feature_branch to the main project's **dev** branch - Click **compare across forks** [here](https://github.com/vnpy/vnpy/compare?expand=1), select the required fork and branch to create PR
7. Wait for review, continue improvements if needed, or get merged!
When submitting code, please follow these rules to improve code quality:
  * Use [ruff](https://github.com/astral-sh/ruff) to check your code style, ensure no errors or warnings. Run ```ruff check .``` in the project root directory.
  * Use [mypy](https://github.com/python/mypy) for static type checking to ensure correct type annotations. Run ```mypy vnpy``` in the project root directory.
## Other Content
* [Get Help](https://github.com/vnpy/vnpy/blob/dev/.github/SUPPORT.md)
* [Community Code of Conduct](https://github.com/vnpy/vnpy/blob/dev/.github/CODE_OF_CONDUCT.md)
* [Issue Template](https://github.com/vnpy/vnpy/blob/dev/.github/ISSUE_TEMPLATE.md)
* [PR Template](https://github.com/vnpy/vnpy/blob/dev/.github/PULL_REQUEST_TEMPLATE.md)
## License
MIT
```
