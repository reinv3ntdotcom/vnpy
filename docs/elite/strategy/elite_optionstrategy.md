# Option Strategy Trading

OptionStrategy is a functional module for **option strategy trading**. Users can conveniently complete tasks such as strategy initialization, starting, stopping, parameter editing, and removal through its UI interface.

## Main Advantages

The OptionStrategy module, designed for complex option strategies, provides the ContractManager component for obtaining contract information data, loading full contract data day by day according to trading days for playback, and offers data structures for multi-layer option data mapping caching.

## Starting the Module

The OptionStrategy module needs to be loaded through the [Strategy Application] tab before starting.

After starting and logging into VeighNa Elite Trader, connect to the trading interface before starting the module. Start the module only after seeing the "Contract information query successful" output in the [Log] column of the VeighNa Elite Trader main interface.

Please note that the IB interface cannot automatically obtain all contract information upon login; it can only be obtained when the user manually subscribes to market quotes. Therefore, manually subscribe to contract quotes on the main interface first, then start the module.

After successfully connecting to the trading interface, click [Functions] -> [Option Strategy Trading] in the menu bar, or click the icon in the left button bar:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/1.png)

This will enter the UI interface of the option strategy trading module.

If data services are configured, opening the multi-process portfolio strategy module will automatically perform data service login initialization. If the login is successful, it will output the "Data service initialization successful" log, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/2.png)

## Strategy File Directory

<span id="jump">

For user-developed strategies, they need to be placed in the **strategies** directory under the VeighNa Elite Trader runtime directory to be recognized and loaded. The specific runtime directory path can be viewed in the title bar at the top of the VeighNa Elite Trader main interface.

For users with default installation on Windows, the strategies directory path for placing strategies is usually:

```
C:\Users\Administrator\strategies
```

Where Administrator is the current logged-in Windows system username.

</span>

## Creating Strategy Instances

Users can create different strategy instances (objects) based on well-written portfolio strategy templates (classes).

In the drop-down box in the upper left corner, select the strategy name to trade, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/3.png)

Please note that the displayed strategy name is the name of the **strategy class** (camel case naming), not the name of the strategy file (underscore mode naming).

After selecting the strategy class, click [Add Strategy], and the Add Strategy dialog box will pop up, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/4.png)

When creating a strategy instance, relevant parameters need to be configured, with the following requirements for each parameter:

- Instance Name
  - Instance names cannot be duplicated;
- Parameter Settings
  - The displayed parameter names are parameters defined using the Parameter helper class in the strategy, as shown below:
    - Option product code (must be the option product name on the contract that can be queried in the live trading system)
    - Order overprice ratio
  - Default values are the default values of the parameters in the strategy;
  - As can be observed from the figure above, the <> brackets after the parameter name display the data type of the parameter. When filling in parameters, follow the corresponding data types. Among them, <class 'str'> is string, <class 'int'> is integer, <class 'float'> is float;
  - Please note that if a parameter may be adjusted to a value with decimal places, and the default parameter value is an integer (e.g., 1). When writing the strategy, set the default parameter value to a float (e.g., 1.0). Otherwise, the strategy will default the parameter to an integer, and when [editing] the strategy instance parameters later, only integers will be allowed.

After parameter configuration is complete, click the [Add] button to start creating the strategy instance. After successful creation, the strategy instance can be seen in the strategy monitoring component on the left, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/5.png)

The top of the strategy monitoring component displays the strategy instance name, strategy class name, and strategy author name (author defined in the strategy). The top buttons are used to control and manage the strategy instance. The first row table displays the internal parameter information of the strategy (parameter names need to be defined using the Parameter helper class in the strategy for display in the graphical interface), and the second row table displays the variable information during the strategy operation (variable names need to be defined using the Variables helper class in the strategy for display in the graphical interface). The [inited] field indicates the current initialization status of the strategy (whether historical data playback has been completed), and the [trading] field indicates whether the strategy can currently start trading.

As can be observed from the figure above, at this time, the [inited] and [trading] states of the strategy instance are both [False]. This indicates that the strategy instance has not been initialized and cannot yet send trading signals.

After the strategy instance is created successfully, the configuration information of the strategy instance will be saved to the option_strategy_setting.json file in the .vntrader folder.

## Initializing the Strategy

After the strategy instance is created successfully, the instance can be initialized. Click the [Initialize] button under the strategy instance. If initialization is successful, it will be as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/6.png)

After initialization is complete, it can be observed that the [inited] state of the strategy instance is now [True]. This indicates that the strategy instance has completed initialization. The [trading] state is still [False], indicating that the strategy instance cannot yet start automatic trading.

## Starting the Strategy

Only when the strategy instance is initialized successfully and the [inited] state is [True] can the automatic trading function of the strategy be started. Click the [Start] button under the strategy instance to start the strategy instance. After success, it will be as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/7.png)

It can be observed that the [inited] and [trading] states of the strategy instance are both [True]. This indicates that the strategy instance has completed the subscription to market quotes for the contracts specified in the initialization function, and at this time, the strategy's internal trading request functions (buy/sell/short/cover/cancel_order, etc.) and information output functions (send_email/put_event, etc.) will actually execute and send corresponding request instructions to the underlying interface (actually execute trades).

In the previous step of strategy initialization, although the strategy is also receiving (historical) data (if load_bars function is called in the initialization function) and calling corresponding functions, because the [trading] state is [False], there will be no actual order placement operations or trade-related log information output.

If orders are sent after starting, you can go to the [Orders] column in the VeighNa Elite Trader main interface to view order details.

## Stopping the Strategy

If, after starting the strategy, you want to stop, edit, or remove the strategy due to certain situations (such as market closing time or emergencies during the session), you can click the [Stop] button under the strategy instance to stop the automatic trading of the strategy instance. After success, it will be as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/8.png)

The option strategy engine will automatically cancel all active orders sent by the strategy before to ensure no uncontrolled orders exist after the strategy stops. At the same time, the latest variable information of the strategy instance will be saved to the option_strategy_data.json file in the .vntrader folder.

At this time, it can be observed that the [trading] state of the strategy instance has changed to [False], indicating that the strategy instance has stopped automatic trading.

In the live trading process of option strategies, under normal circumstances, the strategy should be allowed to run automatically throughout the trading session, and extra pause and restart operations should be avoided as much as possible. For the domestic futures market, the automatic trading of the strategy should be started before the trading session begins, and then closed after the market closes. Because the CTP night session also shuts down the system after closing, and restarts before the morning opening, the strategy needs to be stopped after the night session closes, and VeighNa Elite Trader closed.

## Editing the Strategy

If, after creating a strategy instance, you want to edit the parameters of a strategy instance (if the strategy has been started, first click the [Stop] button under the strategy instance to stop the strategy), you can click the [Edit] button under the strategy instance, and the parameter editing dialog box will pop up for modifying strategy parameters. As shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/9.png)

After editing the strategy parameters, click the [Confirm] button below, and the corresponding changes will be immediately updated in the parameter table, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/10.png)

However, the trading contract code of the strategy instance cannot be modified, and the initialization operation will not be re-executed after modification. Also note that at this time, only the parameter values of the strategy instance in the option_strategy_setting.json file under the .vntrader folder are modified, not the parameters under the original strategy file.

If you want to start the strategy again after editing during the session, click the [Start] button under the strategy instance to start the strategy instance again, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/11.png)

## Removing the Strategy

If, after creating a strategy instance, you want to remove a strategy instance (if the strategy has been started, first click the [Stop] button under the strategy instance to stop the strategy), you can click the [Remove] button under the strategy instance. After successful removal, the strategy monitoring component on the left of the graphical interface will no longer display the information of the strategy instance, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/12.png)

At this time, the configuration information of the strategy instance is also removed from the option_strategy_setting.json file under the .vntrader folder.

## Status Tracking

If you want to track the strategy status through the graphical interface, there are two ways:

1. Call the put_event function

   All variable information in the strategy instance needs to have the variable names written in the strategy's variables list to be displayed in the graphical interface. If you want to track changes in variable status, you need to call the put_event function in the strategy for data refresh on the interface.

   Sometimes users may find that no matter how long their written strategy runs, the variable information does not change. In this case, please check if the call to the put_event function is missing in the strategy.

2. Call the write_log function

   If you not only want to observe changes in variable information but also output personalized logs based on your needs according to the strategy status, you can call the write_log function in the strategy for log output.

## Running Logs

### Log Content

The logs output on the UI interface of the option strategy module come from two sources: the strategy engine and the strategy instance.

**Engine Logs**

The strategy engine generally outputs global information. In the figure below, except for the content after the strategy instance name followed by a colon, all are logs output by the strategy engine.

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/13.png)

**Strategy Logs**

If the write_log function is called in the strategy, the log content will be output through the strategy log. The content in the red box in the figure below is the strategy log output by the strategy instance. Before the colon is the name of the strategy instance, and after the colon is the content output by the write_log function.

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/14.png)

### Clear Operation

If you want to clear the log output on the option strategy UI interface, you can click the [Clear Logs] button in the upper right corner to clear all logs output on the interface with one click.

Before clicking [Clear Logs], as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/12.png)

After clicking [Clear Logs], as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/optionstrategy/15.png)

## Batch Operations

After the strategy has been fully tested and runs stably in live trading without frequent adjustments, if there are multiple portfolio strategy instances to run, you can use the [Initialize All], [Start All], and [Stop All] functions in the upper right corner of the interface to perform batch initialization before the session, start strategy instances, and batch stop strategy instances after the session.

## Option Strategy Template

The option strategy template provides signal generation and order management functions, and users can develop option strategies based on this template.

User-developed strategies can be placed in the [strategies](#jump) folder under the user runtime folder.

Please note:
   - Strategy file naming uses underscore mode, such as io_strategy.py, while strategy class naming uses camel case, such as IoStrategy.

   - The class name of self-built strategies should not overlap with the class names of example strategies. If they overlap, only one strategy class name will be displayed on the graphical interface.

### StrategyTemplate

VeighNa Elite Trader's option strategy trading module provides the StrategyTemplate professional option strategy template to implement complex option strategy development.

### Functions Called by the Strategy Engine

The update_setting function in StrategyTemplate and the three functions starting with get after it (get_parameters, get_variables, and get_data), as well as the update_trade and update_order functions, are functions called by the strategy engine and generally do not need to be called when writing strategies.

### Strategy Callback Functions

Functions starting with on in StrategyTemplate are called callback functions, which can be used to receive data or status updates during strategy writing. The role of callback functions is that when a certain event occurs, such functions in the strategy will be automatically called by the option strategy engine (no need to actively operate in the strategy). Callback functions can be divided into the following two categories based on their functions:

#### Strategy Instance Status Control (Required for All Strategies)

**on_init**

* Input: None

* Output: None

The on_init function is called when initializing the strategy. The default writing is to first call the write_log function to output the "Strategy initialization" log, then call the subcribe_options function to subscribe to option quotes (if needed, also call subscribe_data function to subscribe to underlying quotes).

If there are variables that need global caching, the cache containers can be defined in the on_init function.

If historical data needs to be read for indicator calculation, the load_bars function can also be called to load historical data and push it.

When the strategy is initialized, both the inited and trading states of the strategy are [False], and trading signals cannot be sent. After calling the on_init function, the inited state of the strategy becomes [True], and the strategy initialization is complete.

**on_start**

* Input: None

* Output: None

The on_start function is called when starting the strategy. The default writing is to call the write_log function to output the "Strategy started" log.

After calling the strategy's on_start function to start the strategy, the trading state of the strategy becomes [True], and at this time, the strategy can send trading signals.

**on_stop**

* Input: None

* Output: None

The on_stop function is called when stopping the strategy. The default writing is to call the write_log function to output the "Strategy stopped" log.

After calling the strategy's on_stop function to stop the strategy, the trading state of the strategy becomes [False], and at this time, the strategy will not send trading signals.

#### Receiving Data, Calculating Indicators, Sending Trading Signals

**on_tick**

* Input: tick: TickData

* Output: None

When the strategy receives the latest tick data push in live trading, the on_tick function is called. The default writing is to first decide the execution frequency of the strategy (can filter by judging the tick's datetime), then periodically push the cached tick price data into the encapsulated strategy execution function, and finally cache the newly received tick data into the data cache container.

Please note that the on_tick function is only called in live trading and is not supported in backtesting.

**on_bars**

* Input: bars: Dict[str, BarData]

* Output: None

When the strategy receives the latest bar data during backtesting, the on_bars function is called. The default writing is to push the received bar price data into the encapsulated strategy execution function.

Please note that the on_bars function is only called in backtesting and is not supported in live trading.

The option strategy module, when receiving bar pushes, receives all contract bar data at that time point at once through the on_bars callback function, rather than receiving them one by one through the on_bar function.

#### Order Status Updates

Because option strategies require simultaneous ordering and trading of multiple contracts, during backtesting, it is impossible to determine the sequence of order executions for each contract within a bar segment, so on_order and on_trade functions cannot be provided to get order and trade pushes. Status queries can only be performed through get_pos during callbacks.

### Active Functions

**buy**: Buy to open (Direction: LONG, Offset: OPEN)

**sell**: Sell to close (Direction: SHORT, Offset: CLOSE)

**short**: Sell to open (Direction: SHORT, Offset: OPEN)

**cover**: Buy to close (Direction: LONG, Offset: CLOSE)

* Input: vt_symbol: str, price: float, volume: float

* Output: vt_orderids: List[str] / None

buy/sell/short/cover are internal trading request functions responsible for sending orders in the strategy. The strategy can send trading signals to the strategy engine through these functions to achieve ordering.

Taking the buy function code below as an example, you can see that the **specific contract code to trade**, price, and volume are required parameters, while lock conversion and net position conversion default to False. You can also see that after receiving the passed parameters, the function internally calls the send_order function in StrategyTemplate to send the order (since it is a buy command, the direction is automatically filled as LONG, and offset as OPEN).

Please note that if a close order is sent to the Shanghai Futures Exchange, because the exchange must specify close today or close yesterday, the underlying will automatically convert the close order. Because some varieties on the Shanghai Futures Exchange have close today discounts, orders are sent by default with close today priority (if the traded underlying has better close yesterday discounts on the Shanghai Futures Exchange, appropriate modifications can be made in the convert_order_request_shfe function in vnpy.trader.converter).

**send_order**

* Input: vt_symbol: str, direction: Direction, offset: Offset, price: float, volume: float

* Output: vt_orderids: List[str] / None

The send_order function is the function called by the strategy engine to send orders. Generally, it does not need to be called separately when writing strategies.

In live trading, after receiving the passed parameters, the round_to function will be called to process the order price and volume based on the contract's pricetick and min_volume.

Please note that trading orders can only be sent after the strategy is started, that is, after the strategy's trading state becomes [True]. If this function is called when the strategy's Trading state is [False], it will only return [].

**cancel_order**

* Input: vt_orderid: str

* Output: None

**cancel_all**

* Input: None

* Output: None

cancel_order and cancel_all are trading request functions responsible for canceling orders. cancel_order cancels specific active orders in the strategy, and cancel_all cancels all active orders of the strategy. Generally, they do not need to be called separately when writing strategies; when calling the execute_trading function, cancel_all will be automatically called to batch cancel sent active orders.

Please note that orders can only be canceled after the strategy is started, that is, after the strategy's trading state becomes [True].

**get_porftfolio**

* Input: portfolio_name: str

* Output: portfolio: PortfolioData

Calling the get_porftfolio function in the strategy can obtain the option portfolio of a specific option product.

**subscribe_options**

* Input: portfolio_name: str

* Output: res: bool

Calling the subscribe_options function in the strategy can subscribe to the option portfolio quotes of a specific option product.

The option portfolio name must be the option product name on the contract that can be queried in the live trading system (can be viewed through [Contract Query]).

    Examples of underlying corresponding option product names:

    ETF options
      "510050" - "510050_O"
      "159919" - "159919_O"

    Index options
      "IF" - "IO"
      "IH" - "HO"
      "IM" - "MO"

    Commodity options
      "i" - "i_o"
      "cu" - "cu_o"
      "sc" - "sc_o"
      "SR" - "SR"

If False is returned, it means the underlying did not obtain the corresponding option contract information.

**subscribe_data**

* Input: vt_symbol: str

* Output: res: bool

Calling the subscribe_data function in the strategy can subscribe to specific contract quotes.

If False is returned, it means the underlying did not obtain the corresponding contract information.

### Utility Functions

The following are utility functions outside the strategy:

**get_pos**

* Input: vt_symbol: str

* Output: int / 0

Calling the get_pos function in the strategy can obtain the position data of a specific contract.

**write_log**

* Input: msg: str

* Output: None

Calling the write_log function in the strategy can output logs with specified content.

**load_bars**

* Input: vt_symbol: str, days: int, interval: Interval

* Output: bars: List[BarData] / None

Calling the load_bars function in the strategy can obtain bar data of a specific contract during strategy class initialization.

When called in live trading, the load_bars function will first try to obtain historical data through the trading interface, data service, and database in sequence until historical data is obtained or empty is returned.

**put_event**

* Input: None

* Output: None

Calling the put_event function in the strategy can notify the graphical interface to refresh the strategy status-related display. It can be called in the encapsulated strategy execution function.

Please note that the interface can only be refreshed after the strategy initialization is complete and the inited state becomes [True].

**send_email**

* Input: msg: str

* Output: None

After configuring email-related information (for configuration methods, see the global configuration section in the basic usage chapter), calling the send_email function in the strategy can send emails with specified content to your own email.

Please note that emails can only be sent after the strategy initialization is complete and the inited state becomes [True].

**sync_data**

* Input: None

* Output: None

Calling the sync_data function in the strategy can synchronize strategy variables into a json file for local caching every time it stops or trades in live trading, facilitating reading and restoration upon initialization the next day (the strategy engine will call it, no need to actively call it in the strategy).

Please note that strategy information can only be synchronized after the strategy is started, that is, after the strategy's trading state becomes [True].

**save_data**

* Input: file_name: str, data: dict

* Output: None

Calling the save_data function in the strategy can save strategy data to a specified file.

**load_data**

* Input: file_name: str

* Output: None

Calling the load_data function in the strategy can load strategy data from a specified file.

**get_today**

* Input: None

* Output: None

Calling the get_today function in the strategy can obtain the current date.

### Position Target Adjustment Trading Utility Functions

The following are utility functions called by the strategy in position target adjustment trading mode:

**set_target**

* Input: vt_symbol: str, target: int

* Output: None

Calling the set_target function in the strategy can set the target position for a specific contract.

Please note: The target position is a persistent state, so it will remain in subsequent times after setting until it is modified again.

**get_target**

* Input: vt_symbol: str

* Output: int

Calling the get_target function in the strategy can obtain the set target position for a specific contract.

Please note: The target position state of the strategy will be automatically persisted to a disk file during sync_data (upon trade, stop, etc.) and restored after strategy restart.

**clear_targets**

* Input: None

* Output: int

Calling the clear_targets function in the strategy can clear the cached contract target positions.

**execute_trading**

* Input: price_data: Dict[str, float], percent_add: float

* Output: None

Calling the execute_trading function in the strategy can execute position adjustment trading based on the set target positions for specific contracts.

execute_trading is a function that executes trades based on the set target positions and overprice percentage. Order placement and cancellation have been taken over by this function, no need to perform order placement and cancellation operations in the strategy.

After execute_trading is called, it will first cancel all active orders of the strategy internally, then place orders based on the position difference between the strategy target and strategy position (no order if none).

Please note: Only contracts sliced in the current price_data will participate in this position adjustment trading execution, thus ensuring that contracts in non-trading sessions (no quote pushes) will not erroneously send orders.

### Strategy Data Containers

#### OptionData (Option Data)

##### Properties

 - vt_symbol: str (Local code)
 - contract: ContractData (Contract information)
 - strike: float (Strike price)
 - price: float (Latest price)
 - pos: float (Net position)

#### ChainData (Option Chain Data)

##### Properties

 - symbol: str (Underlying contract code)
 - exchange: Exchange (Exchange)
 - expiry: datetime (Option expiration date)
 - strikes: List[float] (Option strike prices)
 - calls: Dict[float, OptionData] (Call options)
 - puts: Dict[float, OptionData] (Put options)
 - atm_strike: float (At-the-money option strike price)

##### Functions

- **add_contract**

  * Input: contract: ContractData

  * Output: option: OptionData

  Calling the add_contract function can add specified option contract information to the calls, puts, and strikes of the option chain instance.

  Generally, ChainData's add_contract is only called when PortfolioData calls add_contract. No need to call this function in the strategy.

- **calculate_atm**

  * Input: underlying_price: float

  * Output: None

  Calling the calculate_atm function can calculate and update the atm_strike property of ChainData for the at-the-money option strike price (if no underlying is passed, use synthetic futures calculation. Otherwise, use the underlying contract price for calculation).

  Generally in the strategy, after the encapsulated strategy execution function receives price data, it obtains the specified portfolio and calls the update_price function of the PortfolioData instance to update the price to the option portfolio. Then, the calculate_atm function of the specified ChainData instance can be called to calculate the at-the-money option strike price.

- **calculate_synthetic**

  * Input: None

  * Output: synthetic_price: float

  After the ChainData instance has atm_strike, calling the calculate_synthetic function can calculate the synthetic futures price.

- **get_option_by_level**

  * Input: cp: int, level: int

  * Output: option: OptionData

  After the ChainData instance has atm_strike, calling the get_option_by_level function can query options based on the position of the at-the-money option on the option chain according to the passed out-of-the-money level.

  Please note: cp greater than 0 queries call options, cp less than 0 queries put options. level is the out-of-the-money level of the option.

#### PortfolioData

##### Properties

 - symbol: str (Option product code)
 - exchange: Exchange (Exchange)
 - chains: Dict[str, ChainData] (Option chains)
 - chain_symbols: List[str] (Contract codes on each option chain)
 - options: Dict[str, OptionData] (Options)

- **add_contract**

  * Input: contract: ContractData

  * Output: None

  Calling the add_contract function can add specified option contract information to the calls, puts, strikes of the option chain instance on the option portfolio instance, and to the options dictionary of the option portfolio instance.

- **get_chain_by_level**

  * Input: level: int

  * Output: chains: List[ChainData] / None

  Calling the get_chain_by_level function can query option chains by month.

  Passing 0 for the level parameter queries the current month option chain.

- **update_price**

  * Input: price_data: dict[str, float]

  * Output: None

  Calling the update_price function can update the received prices to the specified options.

  Generally, the key of price_data is set to the vt_symbol of the contract.

- **update_pos**

  * Input: pos_data: dict[str, int]

  * Output: None

  Calling the update_pos function can update the strategy's position data to the specified options.

  Generally, the key of pos_data is set to the vt_symbol of the contract.

### Strategy Available Bar Synthesizer

#### OptionBarGenerator - Option Strategy Bar Cross-Section Synthesizer

OptionBarGenerator is a bar cross-section synthesis tool designed specifically for option strategies, used to convert real-time tick data into 1-minute bar data cross-sections. This tool supports simultaneous processing of multiple contracts and can efficiently provide the required bar data for option strategies.

##### Functional Features

 - Multi-contract support: Simultaneously process tick data of multiple contracts and generate corresponding bar data
 - Timestamp monitoring: Can specify a specific contract as the timestamp monitoring contract to control the rhythm of bar generation
 - Trading session filtering (only supports futures and futures options): Automatically filter data from non-trading sessions based on configured varieties and sessions to ensure bar quality
 - Real-time updates: Update bar information in real-time as tick data is pushed

##### Usage

```python
from elite_optionstrategy.utility import OptionBarGenerator

class MyOptionStrategy(StrategyTemplate):
    
    def __init__(self, strategy_engine, strategy_name, vt_symbols, setting):
        super().__init__(strategy_engine, strategy_name, vt_symbols, setting)
        
        # Create bar generator instance
        # Optionally specify timestamp monitoring contract
        self.bg = OptionBarGenerator(self.on_bars, dt_symbol="xxx")
        
    def on_tick(self, tick: TickData):
        """Tick data update"""
        # Update bar generator
        self.bg.update_tick(tick)
        
        # Other tick data processing logic
        
    def on_bars(self, bars: dict[str, BarData]):
        """Bar cross-section data update"""
        # Process bar cross-section data
        for vt_symbol, bar in bars.items():
            print(f"{vt_symbol} - High:{bar.high_price} Open:{bar.open_price} Low:{bar.low_price} Close:{bar.close_price}")
        
        # Strategy signal calculation and trading logic
```

Parameter Description
 - on_bars: Callback function, called when a new bar cross-section is generated, receiving a dictionary containing bars of multiple contracts
 - dt_symbol: Specify the timestamp monitoring contract code, used to control the rhythm of bar generation (synthesize all contracts' previous minute bars upon receiving the next minute tick of this contract), leave blank if not specified

Working Principle
OptionBarGenerator updates its internal state by continuously receiving tick data. When a minute change is detected, it generates the current minute's bar cross-section and returns it through the callback function. If a timestamp monitoring contract is specified, the bar cross-section generation is only triggered when the timestamp of that contract changes by a minute. If a timestamp monitoring contract is specified and contract filtering information is configured (refer to [Market Data Filtering Documentation](https://www.vnpy.com/docs/cn/elite/strategy/elite_filter.html)), the bar cross-section generation is only triggered when the timestamp of that contract changes by a minute and is within a valid trading session.

## Strategy Backtesting

The reference code for strategy backtesting in the option strategy trading module is as follows:

```
engine = BacktestingEngine()

engine.set_parameters(
    interval=Interval.MINUTE,
    start=datetime(2023, 1, 1),
    end=datetime(2023, 11, 30),
    rate=0,
    slippage=0,
)

engine.add_strategy(IoStrategy, {})
engine.run_backtesting()
engine.calculate_result()
engine.calculate_statistics()
engine.show_chart()
```

- **set_parameters**

  * Input: interval: Interval,  start: datetime, end: datetime, rate: float, slippage: float, capital: int = 1_000_000, cache: str = "", memory: bool = False

  * Output: None

  Calling the set_parameters function can set the external backtesting parameters of the BacktestingEngine instance.

- **add_strategy**

  * Input: strategy_class: Type[StrategyTemplate], setting: dict

  * Output: None

  Calling the add_strategy function can add the option strategy instance and strategy settings to the BacktestingEngine instance.

- **run_backtesting**

  * Input: disable_tqdm: bool= False

  * Output: None

  Calling the run_backtesting function can execute backtesting tasks day by day.

  When calling run_backtesting, the BacktestingEngine instance will load the full contract data for the day according to trading days for playback, and simulate live situations by re-initializing the strategy object instance daily.

  - Intraday backtesting process: First obtain the strategy position and price data at yesterday's close, then update the data to the daily statistical results. Then create a new strategy instance, restore the strategy position and execute strategy initialization. At this time, historical data can be loaded according to subscription (all contracts subscribed by the strategy), then execute strategy start and data playback (process order matching and historical data pushes). After playback, execute strategy stop, obtain the closing strategy position and update the closing data to the statistical results.

- **calculate_result**

  * Input: None

  * Output: None

  Calling the calculate_result function can calculate the daily mark-to-market profit and loss of the strategy instance.

- **calculate_statistics**

  * Input: df: DataFrame = None, output: bool = True

  * Output: None

  Calling the calculate_statistics function can calculate and output the statistical indicators of the strategy instance based on the daily mark-to-market profit and loss of the strategy instance.

- **show_chart**

  * Input: df: DataFrame = None

  * Output: None

  Calling the show_chart function can display charts based on the backtesting results of the strategy instance.

### Option Backtesting Data Caching

To speed up the backtesting and optimization of option strategies, the backtesting engine supports data file caching: When calling the set_parameters function, pass the backtesting task cache name parameter cache (str type), and data caching will be automatically created during the first backtesting, speeding up each subsequent backtesting.

- **list_cache**

  * Input: None

  * Output: list

  Calling the list_cache function can return the list of backtesting data caches.

- **remove_cache**

  * Input: cache: str

  * Output: list

  Calling the remove_cache function can delete the specified cache data.
