# CTA Trend Strategy

CtaStrategy is a functional module for **CTA automated trading**. Users can conveniently complete tasks such as strategy initialization, starting, stopping, parameter editing, and removal through its UI interface.

## Main Advantages

The CtaStrategy module fully utilizes multi-core CPUs and supports multi-process CTA strategy trading. It also provides a professional CTA strategy template, EliteCtaTemplate, to enable more powerful CTA strategy development.

To address inconsistencies between backtesting and live trading, EliteCtaTemplate includes a built-in scheme for [maintaining strategy operation based on theoretical targets](#jump1). Additionally, EliteCtaTemplate supports filtering configurations for junk data during non-trading periods (refer to the filtering configuration section for details).

## Starting the Module

The CtaStrategy module needs to be loaded via the [Strategy Application] tab before starting.

After starting and logging into VeighNa Elite Trader, connect to the trading interface before launching the module. Proceed to start the module only after seeing the "Contract information query successful" output in the [Log] section of the VeighNa Elite Trader main interface.

Note that the IB interface cannot automatically retrieve all contract information upon login; it only obtains it when users manually subscribe to market quotes. Therefore, manually subscribe to contract quotes on the main interface first, then start the module.

After successfully connecting to the trading interface, click [Functions] -> [Multi-Process CTA Trading] in the menu bar, or click the icon in the left button bar:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/1.png)

This will enter the UI interface of the multi-process CTA trading module.

If data services are configured, the CTA strategy module will automatically perform data service login initialization upon opening. If login is successful, it will output the log "Data service initialization successful," as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/2.png)

## Strategy File Directory

<span id="jump">

For user-developed strategies, they need to be placed in the **strategies** directory under the VeighNa Elite Trader runtime directory to be recognized and loaded. The specific runtime directory path can be viewed in the title bar at the top of the VeighNa Elite Trader main interface.

For users with default installation on Windows, the strategies directory path for placing strategies is typically:

```
C:\Users\Administrator\strategies
```

Where Administrator is the current logged-in Windows system username.

</span>

## Creating Strategy Instances

Users can create different strategy instances (objects) based on well-written CTA strategy templates (classes). The advantage of strategy instances is that the same strategy can trade multiple contract varieties simultaneously, and each instance can have different parameters.

In the upper-left dropdown box, select the strategy name to trade, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/3.png)

Note that the displayed strategy name is the **strategy class** name (camel case), not the strategy file name (underscore naming).

After selecting the strategy class, click [Add Strategy], and the Add Strategy dialog box will appear, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/4.png)

When creating a strategy instance, relevant parameters need to be configured, with the following requirements:

- Instance Name
  - Instance names cannot be duplicated;
- Contract Variety
  - Format is vt_symbol (contract code + exchange name);
  - Must be a contract name that can be queried in the live trading system;
  - Generally, select the month with the best liquidity for the futures variety;
- Interface Name
  - Select the interface name for trading;
- Parameter Settings
  - The displayed parameter names are those defined using the Parameter helper class in the strategy;
  - Default values are the default values of the parameters in the strategy;
  - As observed in the figure above, the parameter name is followed by <> brackets showing the data type of the parameter. When filling in parameters, follow the corresponding data type. Where <class 'str'> is string, <class 'int'> is integer, <class 'float'> is float;
  - Note that if a parameter may be adjusted to a value with decimal places, and the default parameter value is an integer (e.g., 1), set the default parameter value to a float (e.g., 1.0) when writing the strategy. Otherwise, the strategy will default the parameter to integer, and when [Editing] strategy instance parameters later, only integers will be allowed.

After parameter configuration is complete, click the [Add] button to start creating the strategy instance. Upon successful creation, the strategy instance can be seen in the left strategy monitoring component. Since each strategy is an independent process, the graphical interface will output the log "Strategy process started" after successful addition, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/5.png)

The top of the strategy monitoring component displays the strategy instance name, interface name, contract variety name, strategy class name, and strategy author name (defined as author in the strategy). The top buttons are used to control and manage the strategy instance. The first row table displays the internal parameter information of the strategy (parameter names need to be written in the strategy's parameters list to be displayed in the graphical interface), and the second row table displays variable information during strategy operation (variable names need to be written in the strategy's variables list to be displayed in the graphical interface). The [inited] field indicates the current initialization status of the strategy (whether historical data playback is completed), and the [trading] field indicates whether the strategy can start trading.

As observed in the figure above, at this point, the [inited] and [trading] states of the strategy instance are both [False]. This indicates that the strategy instance has not been initialized and cannot issue trading signals yet.

After successful creation of the strategy instance, its configuration information will be saved to the cta_strategy_setting.json file in the .vntrader folder.

## Initializing Strategy

After successful creation of the strategy instance, it can be initialized. Click the [Initialize] button under the strategy instance. If initialization is successful, it will be as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/6.png)

During initialization, the following three tasks are completed in sequence:

1. Retrieve Historical Data

   To ensure the accuracy of indicator values in the strategy, each strategy instance requires a certain amount of historical data for initialization.

   Therefore, during strategy initialization, the load_bar function inside the strategy instance will first retrieve the latest historical data from the interface. If the interface does not provide historical data, it will be obtained through the configured data service ([RQData](https://www.ricequant.com/welcome/purchase?utm_source=vnpy) provides historical data for domestic futures, stocks, and options. RQData's data service provides intraday K-line updates, so even if the strategy is started at 9:45, it can retrieve K-line data from 9:30 opening to 9:45 for initialization calculations without worrying about data missing).

   The specific length of data loaded depends on the load_bar function's parameter control (default is 10 days in the strategy template). After data loading, it will be pushed to the strategy bar by bar (or tick), to initialize internal variables, such as caching K-line sequences, calculating technical indicators, etc.

2. Load Cached Variables

   During daily live operation, some variables in quantitative strategies are only related to historical market data, and these can be correctly valued by loading historical data playback. Other variables may be related to trading status, such as strategy positions, which need to be cached on the hard drive (upon program exit), and restored after historical data playback the next day to ensure consistency with previous trading status.

   Each time the strategy is stopped, the variables corresponding to the strategy's variables list and strategy positions will be automatically cached in the cta_strategy_data.json file under the .vntrader directory, for automatic loading during the next strategy initialization.

   Note that in some cases (e.g., manual closing), cached data may have discrepancies (because strategy position maintenance is the logical position of the running strategy instance, not the position of a specific variety), which can be adjusted by manually modifying the json file.

3. Subscribe to Market Quotes

   Finally, based on the vt_symbol parameter, retrieve the contract information traded by the strategy and subscribe to real-time market quote pushes for that contract. If the live trading system cannot find the contract information, such as not connecting to the login interface or incorrect vt_symbol, corresponding error messages will be output in the log module.

After completing the above three steps, it can be observed that the [inited] state of the strategy instance is now [True], and variables display corresponding values (no longer 0). This indicates that the strategy instance has called the load_bar function to load historical data and complete initialization. The [trading] state is still [False], indicating that the strategy instance cannot start automated trading yet.

## Starting Strategy

Only when the strategy instance is successfully initialized and the [inited] state is [True] can the automated trading function be started. Click the [Start] button under the strategy instance to start it. Upon success, it will be as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/7.png)

It can be observed that both [inited] and [trading] states of the strategy instance are now [True]. This indicates that the strategy instance has called the load_bar function, completed historical data playback, and now the trading request functions (buy/sell/short/cover/cancel_order, etc.) and information output functions (send_email/put_event, etc.) will actually execute and send corresponding request instructions to the underlying interface (actual trading execution).

In the previous strategy initialization step, although the strategy also receives (historical) data and calls corresponding functions, because the [trading] state is [False], there will be no actual order placement operations or trading-related log outputs.

If the strategy issues limit orders after starting, details can be viewed in the [Orders] section of the VeighNa Elite Trader main interface. If the strategy issues local stop orders, details can be viewed in the stop order monitoring component in the upper-right area of the CTA strategy UI interface.

## Stopping Strategy

After starting the strategy, if you want to stop, edit, or remove it due to certain situations (e.g., market close or intraday emergencies), click the [Stop] button under the strategy instance to stop automated trading. Upon success, it will be as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/8.png)

The CTA strategy engine will automatically cancel all active orders issued by the strategy to ensure no uncontrolled orders exist after stopping. At the same time, the latest variable information of the strategy instance will be saved to the cta_strategy_data.json file in the .vntrader folder.

It can be observed that the [trading] state of the strategy instance has changed to [False], indicating that automated trading has stopped.

In the live trading process of CTA strategies, under normal circumstances, the strategy should run automatically throughout the trading session, avoiding extra pause and restart operations. For domestic futures markets, automated trading should start before the trading session begins, and close after market close. Since CTP closes the system after night session close and restarts before morning open, strategies need to be stopped after night close, and VeighNa Elite Trader closed.

## Editing Strategy

After creating a strategy instance, to edit parameters (if started, first click [Stop] to stop), click the [Edit] button under the strategy instance, and the parameter editing dialog will appear for modification, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/9.png)

After editing parameters, click [Confirm] below, and changes will update immediately in the parameter table, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/10.png)

However, the trading contract code of the strategy instance cannot be modified, and initialization will not be re-executed after modification. Note that this only modifies the parameter values of the strategy instance in the cta_strategy_setting.json file under .vntrader, not the original strategy file parameters.

To restart after intraday editing, click [Start] under the strategy instance, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/11.png)

## Removing Strategy

After creating a strategy instance, to remove it (if started, first click [Stop]), click the [Remove] button under the strategy instance. Upon successful removal, the strategy instance information will no longer display in the left strategy monitoring component. Since each strategy is an independent process, the graphical interface will output "Strategy process exited" log after successful removal, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/12.png)

At this point, the cta_strategy_setting.json file under .vntrader also removes the configuration information of the strategy instance.

## Status Tracking

To track strategy status via the graphical interface, there are two ways:

1. Call put_event Function

   All variable information in the strategy instance needs variable names written in the strategy's variables list to display in the graphical interface. To track variable status changes, call the put_event function in the strategy for data refresh on the interface.

   Sometimes users find that their written strategy variables do not change no matter how long it runs; in this case, check if the call to put_event function is missing in the strategy.

2. Call write_log Function

   To not only observe variable status changes but also output personalized logs based on strategy status, call the write_log function in the strategy for log output.

## Running Logs

### Log Content

Logs output on the CTA strategy module UI interface come from two sources: the CTA strategy engine and strategy instances.

**Engine Logs**

The CTA strategy engine generally outputs global information. In the figure below, except for content starting with the strategy instance name in brackets, all are logs output by the CTA strategy engine.

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/13.png)

**Strategy Logs**

If the write_log function is called in the strategy, log content will be output via strategy logs. The content in the red boxes below are strategy logs output by two different strategy instances. The brackets contain the strategy instance name, followed by the write_log function output.

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/14.png)

### Clear Operation

To clear logs on the CTA strategy UI interface, click the [Clear Logs] button in the upper-right corner to clear all output logs on the interface with one click.

Before clicking [Clear Logs], as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/12.png)

After clicking [Clear Logs], as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/15.png)

## Stop Orders

The stop order monitoring component in the upper-right area of the graphical interface is used to track status changes of all local stop orders in the CTA engine.

Since not all interfaces support stop orders, VeighNa provides local stop order functionality. Even if the trading interface does not support exchange stop orders, users can still enable local stop orders by setting the stop parameter to True in the strategy's order functions (buy/sell/short/cover).

VeighNa's local stop orders have three characteristics:

1. Stored on the local computer, invalid after shutdown;
2. Only visible to the trader, no worry about leaking cards;
3. Stop order triggering has delay, causing some slippage.

**Stop Order Information**

After issuing a local stop order, the monitoring component in the upper-right of the graphical interface will display order details.

Local stop orders have three states: [Pending], [Triggered], and [Cancelled], as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/19.png)

When first issued, the stop order is in [Pending] state. Since stop order information is recorded locally and not sent to the exchange, the [Orders] section on the main interface will not change.

Once the stop order's trigger price is hit, to achieve immediate execution, the CTA strategy engine will immediately issue a **limit** order at **limit up/down price** or **best five levels** price (suggest using local stop orders only for contracts with good liquidity). After issuing the limit order, the [Orders] section on the VeighNa Elite Trader main interface will update the order status, the stop order state will change to [Triggered], and the [Limit Order ID] column will fill in the limit order ID.

Note that **the price displayed on the stop order interface is the local stop order trigger price, not the limit order price issued**.

If the stop order is cancelled by the strategy before triggering, the order status will change to [Cancelled].

## Batch Operations

When strategies are fully tested, stable in live operation, and do not require frequent adjustments, if multiple CTA strategy instances need to run, use the [Initialize All], [Start All], and [Stop All] functions in the upper-right corner of the interface for pre-market batch initialization, starting, and post-market batch stopping.

## Manual Contract Rollover

To use the automatic rollover assistant, before strategy initialization, click the [Rollover Assistant] button in the upper-right corner of the CTA strategy UI interface, and the rollover assistant interface will appear, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/20.png)

If the strategy is already initialized, upon opening, the lower-left corner of the rollover assistant interface will output "Strategy already initialized, cannot perform rollover," as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/21.png)

After successfully opening the rollover assistant interface, click [Refresh] to see contract information traded by all strategy instances under the current CTA strategy module, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/22.png)

Now configure the rollover tasks to execute, where:

- Rollover Contract: Enter the local code (vt_symbol) of the new contract to rollover old positions and strategies to;
- Long Rollover: Number of long positions to rollover (cannot exceed displayed total long account position);
- Short Rollover: Number of short positions to rollover (cannot exceed displayed total short account position);
- Single Order Limit: Upper limit of lots per order during algorithmic rollover;
- Order Overprice: Order price overprice pricetick relative to current best opposite price during algorithmic rollover.

After configuration confirmation, click [Execute Rollover] in the upper-right, and the [Execute Rollover Confirmation] window will appear, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/23.png)

Click [OK] to start execution. During rollover, the lower-left corner will output rollover-related information, and the lower-right will display the rollover algorithm. After completion, the upper half of the rollover assistant will be locked (grayed out, unclickable), as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/24.png)

It can be seen that the rollover of all positions and strategies for the rollover contract was completed in almost 1 second.

Since the upper half of the interface is locked after rollover, to view new rollover-able contract information or perform a new round of rollover (can execute rollover per contract or configure all rollover contracts first, then click [Execute Positions] to rollover all strategies at once), click [Refresh] to refresh the interface. After refresh, the "Close Contract" will change to the name of the just successfully rolled over contract.

Note:
  1. - If the rollover contract is filled as the same as the close contract, clicking [Execute Rollover] will pop up [Execute Rollover Failed] window with "Rollover contract and close contract cannot be the same," as shown below:

       ![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/25.png)

     - If the rollover contract is filled as another variety or wrong exchange, clicking [Execute Rollover] will also pop up [Execute Rollover Failed] window with "Rollover contract and close contract varieties inconsistent," as shown below:

       ![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/26.png)

  2. During algorithmic rollover execution (not completed), click [Stop] in the algorithm monitoring part in the lower-right to terminate a rollover algorithm. But after stopping, there is a risk of imbalance (inconsistent rollover results between close and rollover contracts);

  3. Even if the rollover-able position for the close contract is 0, still need to fill the rollover contract name for rollover.

### Rollover Process

The rollover assistant component subscribes to rollover contract quotes based on configured rollover information and starts corresponding rollover algorithms for orders. The rollover price is the current best opposite price plus or minus overprice pricetick, quantity is configured rollover quantity (not exceeding single order limit). After algorithm ends, update strategy trading code and remove previous strategy instance.

### Rollover Effect

Back to the CTA strategy module UI interface, the trading contract name of the corresponding strategy has changed.

Before rollover, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/27.png)

After successful rollover, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/28.png)

Back to VeighNa Elite Trader main interface, detailed rollover orders and trades can also be viewed.

## Multi-Account Support

### Loading

The CTA strategy module supports multi-account batch order trading.

Taking logging into **CTP** interface as example, in the [Trading Interface] tab below the login interface, select CTP interface in the dropdown. Fill custom interface name (e.g., "CTP1", "CTP2") in "Custom Interface," click [Add], fill sub-account configuration, click [Confirm] to load corresponding account interfaces sequentially.

After adding, click [Login] on login interface to log into VeighNa Elite Trader. In menu bar, click [System] -> [Connect xxx] sequentially (xxx is custom interface name, if "CTP1" filled, menu shows [Connect CTP1]), to connect sub-account interface.

After successful connection, VeighNa Elite Trader main interface [Log] component will output login-related information immediately, and users can see corresponding account information, position information, etc.

### CTA Strategy Module Batch Ordering

To batch order via CTA strategy module, when [Add Strategy] on CTA strategy module graphical interface, click [gateway_name] dropdown to select interface, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/16.png)

After successful strategy addition, strategy instance information can be seen in left strategy monitoring component, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/17.png)

After strategy instance issues orders, track orders placed by corresponding interface in [Orders] and [Trades] components on VeighNa Elite Trader main interface, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/ctastrategy/18.png)

**Note**:
 - Currently supports logging in up to 5 trading accounts simultaneously.

## CTA Strategy Templates

CTA strategy templates provide signal generation and order management functions, allowing users to develop CTA strategies based on templates.

User-developed strategies can be placed in the [strategies](#jump) folder under the user runtime folder.

Note:
   - Strategy file naming uses underscore mode, e.g., rumi_strategy.py, while strategy class naming uses camel case, e.g., RumiStrategy;

   - Custom strategy class names should not duplicate example strategy class names. If duplicated, the graphical interface will only display one strategy class name.

### CtaTemplate

VeighNa Elite Trader provides compatibility support for vnpy_ctastrategy's built-in CtaTemplate. Strategies developed via CtaTemplate can also run successfully on VeighNa Elite Trader's CTA strategy module.

### EliteCtaTemplate

VeighNa Elite Trader's CTA strategy module provides the EliteCtaTemplate professional CTA strategy template for more powerful CTA strategy development.

The following uses the RumiStrategy example to demonstrate the specific steps for strategy development:

Before writing strategy logic based on EliteCtaTemplate, load required internal components at the top of the strategy file, as shown in the code below:

```python3
from numpy import ndarray

from elite_ctastrategy import (
    EliteCtaTemplate,
    HistoryManager,
    Parameter,
    Variable,
    sma,
    wma,
    cross_over,
    cross_below,
)
```

Where:
* EliteCtaTemplate is the CTA strategy template provided by Veighna Elite Trader
* HistoryManager is the container for storing historical data provided by Veighna Elite Trader
* Parameter is the data container for storing strategy parameters
* Variable is the data container for storing strategy variables
* sma, wma, cross_over, and cross_below are built-in calculation functions (for a complete list of calculation functions, refer to the built-in indicator calculation functions section)
* ndarray is the class for type declaration of results calculated in the on_history function.

### Strategy Parameters and Variables

Below the strategy class, set the strategy author (author), parameters (parameters), and variables (variables), as shown in the code below:

```python3

    author = "VeighNa Elite Edition"

    # Basic Parameters (Required)
    bar_window: int = Parameter(30)             # K-line window
    bar_interval: int = Parameter("1m")         # K-line interval
    bar_buffer: int = Parameter(100)            # K-line buffer

    # Strategy Parameters (Optional)
    fast_window: int = Parameter(3)             # Fast moving average window
    slow_window: int = Parameter(50)            # Slow moving average window
    rumi_window: int = Parameter(30)            # Moving average deviation window
    max_holding: int = Parameter(100)           # Maximum holding period
    stop_percent: float = Parameter(0.03)       # Conservative stop loss percentage
    risk_window: int = Parameter(10)            # Risk calculation window
    risk_capital: int = Parameter(1_000_000)    # Trading risk capital
    price_add: int = Parameter(5)               # Order price addition

    # Strategy Variables
    trading_size: int = Variable(1)             # Current order quantity
    rumi_0: float = Variable(0.0)               # Current RUMI value
    rumi_1: float = Variable(0.0)               # Previous RUMI value

```

Although strategy parameters and variables belong to the strategy class, parameters are fixed (specified externally by the trader), while variables change with strategy status during trading, so variables only need to be initialized to corresponding basic types initially. For example: integers set to 0, floats to 0.0.

To have the CTA engine display strategy parameters and variables on the UI during operation and save their values on data refresh or strategy stop, create corresponding parameter and variable instances when creating the strategy class.

Note:
 - Parameter and Variable containers only accept parameters or variables in str, int, float, and bool types;

 - Every strategy developed via EliteCtaTemplate needs to create three basic parameters: bar_window (K-line window), bar_interval (K-line interval - currently only supports "1m" and "1h"), and bar_buffer (K-line buffer length for HistoryManager);

 - When bar_interval is "1m", bar_window must be a number divisible by 60 (excluding 60). When bar_interval is "1h", there is no such restriction.

### Strategy Callback Functions

Functions starting with on in EliteCtaTemplate are callback functions, used to receive data or status updates during strategy writing. The role of callback functions is to be automatically called by the CTA strategy engine when an event occurs (no need to actively operate in the strategy). Callback functions can be divided into three categories by function:

#### Strategy Instance Status Control (Required for All Strategies)

**on_init**

* Input: None

* Output: None

The on_init function is called during strategy initialization. Default implementation calls write_log to output "Strategy initialization" log, then calls load_bar to load historical data, as shown in the code below:

```python3
    def on_init(self) -> None:
        """Initialization"""
        self.write_log("Strategy initialization")
        self.load_bar(10)
```

During strategy initialization, both inited and trading states are [False], only calculating and caching related indicators with historical data manager, cannot issue trading signals. After calling on_init, inited state becomes [True], completing initialization.

**on_start**

* Input: None

* Output: None

The on_start function is called when starting the strategy. Default implementation calls write_log to output "Strategy started" log, as shown below:

```python3
    def on_start(self):
        """
        Callback when strategy is started.
        """
        self.write_log("Strategy started")
```

After calling on_start to start, trading state becomes [True], allowing trading signals.

**on_stop**

* Input: None

* Output: None

The on_stop function is called when stopping the strategy. Default implementation calls write_log to output "Strategy stopped" log, as shown below:

```python3
    def on_stop(self):
        """
        Callback when strategy is stopped.
        """
        self.write_log("Strategy stopped")
```

After calling on_stop, trading state becomes [False], no trading signals issued.

#### Receiving Data, Calculating Indicators, Issuing Trading Signals

**on_history**

* Input: hm: HistoryManager

* Output: None

Once the strategy's [HistoryManager](#jump2) initialization is complete, on_history is called when receiving latest K-line data.

The example strategy class RumiStrategy generates CTA signals via 30-minute K-line data returns. It has three parts, as shown in the code below:

```python3
    def on_history(self, hm: HistoryManager) -> None:
        """K-line push"""
        # Calculate moving average arrays
        fast_array: ndarray = sma(hm.close, self.fast_window)
        slow_array: ndarray = wma(hm.close, self.slow_window)

        # Calculate moving average difference
        diff_array: ndarray = fast_array - slow_array
        rumi_array: ndarray = sma(diff_array, self.rumi_window)

        self.rumi_0 = rumi_array[-1]
        self.rumi_1 = rumi_array[-2]

        # Determine crossovers
        long_signal: bool = cross_over(rumi_array, 0)
        short_signal: bool = cross_below(rumi_array, 0)

        # Calculate trading quantity
        self.trading_size = self.calculate_volume(self.risk_capital, self.risk_window, 1000, 1)

        # Get current target
        last_target: int = self.get_target()

        # Initialize new target (default unchanged)
        new_target: int = last_target

        # Execute open signals
        if long_signal:
            new_target = self.trading_size
        elif short_signal:
            new_target = -self.trading_size

        # Close on holding time
        if self.bar_since_entry() >= self.max_holding:
            new_target = 0

        # Protective stop loss close
        close_price = hm.close[-1]

        if last_target > 0:
            stop_price: float = self.long_average_price() * (1 - self.stop_percent)
            if close_price <= stop_price:
                new_target = 0
        elif last_target < 0:
            stop_price: float = self.short_average_price() * (1 + self.stop_percent)
            if close_price >= stop_price:
                new_target = 0

        # Set new target
        self.set_target(new_target)

        # Execute target trading
        self.execute_trading(self.price_add)

        # Push UI update
        self.put_event()
```

- Signal Calculation: Calculate technical indicators using K-line data from latest HistoryManager instance. Such as moving average arrays and differences. First get needed arrays, then calculate via built-in indicator functions;
  
   Note, cross_over and cross_below in the example are boolean functions checking if the indicator crosses above (previous value <= specified, latest > specified) or below (previous >= specified, latest < specified).

- Set Target: After calculating indicators, call calculate_volume to compute order quantity. Then initialize new target based on current target from get_target. Set new target based on indicator values and strategy status;
  
   For RumiStrategy, long_signal/short_signal set open signals, close via holding time and protective stop loss.

   When current target last_target is 0, bar_since_entry is 0, so no close conditions trigger, only open signals if conditions met. When last_target != 0, if new signal same direction, new target same as current, no order on execution. If close triggered, set new target to 0, close on execution. If new signal opposite, set as new target, order on execution.

   Note, after all set new target logic, before execute target trading, set strategy target via set_target to avoid multiple sets in same K-line.

- Execute Target Trading: After setting new target, directly execute target trading and push UI update.

   Note, to refresh indicator values on graphical interface, do not forget to call put_event().

#### Order Status Updates

The following functions can be passed directly in the strategy, with specific logic handled by backtest/live engines. Note, **do not issue order instructions in these functions**.

**on_trade**

* Input: bar: TradeData

* Output: None

on_trade is called on strategy trade returns.

**on_order**

* Input: bar: OrderData

* Output: None

on_order is called on strategy order returns.

### Active Functions

<span id="jump1">

EliteCtaTemplate has a built-in scheme for caching strategy theoretical trade records. In active functions, except for calculate position difference in execute_trading based on strategy target and position difference, other parts are controlled by strategy theoretical targets.

**set_target**

* Input: target: int

* Output: None

set_target sets strategy target net position (understood as desired strategy position quantity). Positive for long, negative for short.

Note, target position is persistent state, remains until reset.

**get_target**

* Input: None

* Output: int

get_target queries strategy target position.

**execute_trading**

* Input: price_add: float

* Output: None

execute_trading executes trading based on set target position. Order placement and cancellation managed by this function, no need in strategy.

After calling execute_trading, internally cancels all active strategy orders, then orders based on target and position difference (no order if none).

**calculate_volume**

* Input: risk_capital: float, risk_window: int, max_volume = 0, min_volume: int = 0

* Output: trading_size: int

calculate_volume calculates risk-adjusted order quantity.

risk_capital is capital for risk-adjusted quantity, risk_window is K-line period window for risk level, max_volume and min_volume limit max/min order quantity.

Note, **risk_window length cannot exceed HistoryManager container length bar_buffer**.

**bar_since_entry**

* Input: None

* Output: int

bar_since_entry gets K-line periods (theoretical) since open maintained by built-in trade manager.

**long_average_price**

* Input: None

* Output: int

long_average_price gets long average price (theoretical) maintained by built-in trade manager.

**short_average_price**

* Input: None

* Output: int

short_average_price gets short average price (theoretical) maintained by built-in trade manager.

**get_account_pos**

* Input: None

* Output: int

get_account_pos gets base account position for strategy (contract code, trading interface), returns net position (long minus short).

**get_pricetick**

* Input: None

* Output: pricetick: float / None

Call get_pricetick in strategy to get trading contract minimum price tick.

**get_size**

* Input: None

* Output: size: int / None

Call get_size in strategy to get trading contract size multiplier.

**get_account**

* Input: None

* Output: size: AccountData / None

Call get_account in strategy to get trading contract account balance.

</span>

### Utility Functions

The following are utility functions outside the strategy:

**write_log**

* Input: msg: str

* Output: None

Call write_log in strategy for specified log output.

**load_bar**

* Input: days: int, interval: Interval = Interval.MINUTE, callback: Callable = None, use_database: bool = False

* Output: None

Call load_bar in strategy to load K-line data during initialization.

As shown below, default loads 10 days, interval minute, i.e., 10 days 1-min K-lines, suggest loading more rather than less. use_database default False, tries trading interface, data service sequentially until data or empty.

**put_event**

* Input: None

* Output: None

Call put_event in strategy to notify graphical interface to refresh strategy status display.

Note, only after initialization complete, inited [True], can refresh.

**send_email**

* Input: msg: str

* Output: None

After email configuration, call send_email in strategy to send specified content to email.

Note, only after initialization complete, inited [True], can send.

**sync_data**

* Input: None

* Output: None

Call sync_data in strategy to sync variables to json file on live stop for cache, for next day init restore (CTA engine calls, no need in strategy).

Note:
   - Only after start, trading [True], can sync;

   - For EliteCtaTemplate strategies, only position pos synced locally, other variables by theoretical values.

### EliteTargetTemplate

VeighNa Elite Trader's CTA strategy module provides EliteTargetTemplate professional CTA strategy template for more powerful CTA strategy development.

Below introduces functions of EliteTargetTemplate.

### Strategy Parameters and Variables

Below strategy class, set author (author), parameters (parameters), variables (variables).

### Class Initialization

__init__ is strategy class constructor, consistent with inherited EliteTargetTemplate.

In this inherited class, init generally three steps:

1. Inherit CTA template via super(), pass CTA engine, strategy name, vt_symbol, parameter settings in __init__. Note CTA engine can be live or backtest, for same code on backtest/live (parameters auto passed by engine on instance creation, no user set).

2. Call BarGenerator: Synthesize 1-min K-lines from Tick via time slices. If needed, synthesize longer periods like 15-min.

3. Call ArrayManager: Convert K-lines like 1-min, 15-min to vectorized time series structure, support talib for indicators.

ArrayManager default length 100, adjust via size param (size not less than indicator period).

### Strategy Callback Functions

on_ functions in EliteTargetTemplate are callbacks for data or updates. Called auto by engine on events. Divided into three categories:

#### Strategy Instance Status Control (Required)

**on_init**

* Input: None

* Output: None

on_init called on init. Default write_log "Strategy initialization", then load_bar historical.

inited trading [False] on init, only calc cache indicators, no signals. After on_init, inited [True], init complete.

**on_start**

* Input: None

* Output: None

on_start called on start. Default write_log "Strategy started".

**on_stop**

* Input: None

* Output: None

on_stop called on stop. Default write_log "Strategy stopped".

After on_stop, trading [False], no signals.

#### Receiving Data, Calculating, Signals

**on_tick**

* Input: tick: TickData

* Output: None

Most systems push Tick only. Even if some push K-lines, arrival slower than Tick, as synthesized first. So live, all strategy K-lines synthesized from received Tick.

on_tick called on latest Tick push. Default BarGenerator update_tick pushes Tick to bg instance for 1-min synthesis.

**on_bar**

* Input: bar: BarData

* Output: None

on_bar called on latest K-line (live default 1-min from Tick, backtest depends on selected interval).

#### Order Updates

Following can pass, logic by backtest/live engines.

**on_trade**

* Input: trade: TradeData

* Output: None

on_trade on trade returns.

**on_order**

* Input: order: OrderData

* Output: None

on_order on order returns.

**on_stop_order**

* Input: stop_order: StopOrder

* Output: None

on_stop_order on stop order returns.

### Active Functions

**buy**: Buy open (Direction: LONG, Offset: OPEN)

**sell**: Sell close (Direction: SHORT, Offset: CLOSE)

**short**: Short open (Direction: SHORT, Offset: OPEN)

**cover**: Cover close (Direction: LONG, Offset: CLOSE)

* Input: price: float, volume: float, stop: bool = False, lock: bool = False, net: bool = False

* Output: vt_orderids: List[vt_orderid] / None 

buy/sell/short/cover are internal trading request functions for orders.

**send_order**

* Input: direction: Direction, offset: Offset, price: float, volume: float, stop: bool = False, lock: bool = False, net: bool = False

* Output: vt_orderids / None

send_order is engine-called send order function. Usually no separate call in strategy.

**cancel_order**

* Input: vt_orderid: str

* Output: None

**cancel_all**

* Input: None

* Output: None

cancel_order cancels specific active order, cancel_all all active.

**set_target**

* Input: target: int

* Output: None

set_target sets target net position. Positive long, negative short.

Note, persistent, remains until reset.

**get_target**

* Input: None

* Output: int

get_target queries target.

**execute_trading**

* Input: price_add: float, bar: BarData

* Output: None

execute_trading executes based on target. Orders/cancels managed, no in strategy.

After call, cancels active, orders on target-position diff (none if no diff).

### Utility Functions

**write_log**

* Input: msg: str

* Output: None

write_log for log output.

**get_engine_type**

* Input: None

* Output: engine_type: EngineType

If different logic backtest/live, get_engine_type for current type judgment.

Note, import "EngineType" at top if using.

**get_pricetick**

* Input: None

* Output: pricetick: float / None

get_pricetick for min price tick.

**get_size**

* Input: None

* Output: size: int / None

get_size for contract multiplier.

**get_account_pos**

* Input: None

* Output: size: int / None

get_account_pos for account position.

**get_account**

* Input: None

* Output: size: AccountData / None

get_account for account balance.

**load_bar**

* Input: days: int, interval: Interval = Interval.MINUTE, callback: Callable = None, use_database: bool = False

* Output: None

load_bar loads K-lines on init.

Default 10 days, minute, suggest more. use_database False, tries interface, service.

**load_tick**

* Input: days: int

* Output: None

load_tick loads Ticks on init.

**put_event**

* Input: None

* Output: None

put_event refreshes UI.

Note, after inited [True].

**send_email**

* Input: msg: str

* Output: None

send_email sends mail after config.

Note, after inited [True].

**sync_data**

* Input: None

* Output: None

sync_data syncs variables on live stop for cache (engine calls).

Note:
   - After trading [True];

   - EliteCtaTemplate only pos synced, others theoretical.

## History Manager

<span id="jump2">

HistoryManager is built-in fixed-length historical data manager in CTA module. All EliteCtaTemplate strategies get K-lines via on_history hm instance for indicators and orders.

Each instance HistoryManager depends on bar_window, bar_interval, bar_buffer. bar_window interval determine time frequency, bar_buffer container length (init success when cached >= bar_buffer).

**datetime**: K-line start time

**open**: K-line open price

**high**: K-line high

**low**: K-line low

**close**: K-line close price

**volume**: K-line volume

**turnover**: K-line turnover

**open_interest**: K-line open interest (no for stocks)

HistoryManager caches datetime, open, high, low, close, volume, turnover, open_interest for synthesized K-lines. For open_price, get array via hm instance (hm.open). Latest open: hm.open[-1].

**bar_count**

* Input: None

* Output: int

If indicators 0 after start, call hm.bar_count() check if < bar_buffer, means short data, not init. Load more or reduce bar_buffer.

**to_dataframe**

* Input: None

* Output: df: pd.DataFrame

To convert hm cached K-lines to DataFrame, call hm.to_dataframe(). Gets length bar_buffer, index datetime, columns open, high, low, close, volume, turnover, open_interest.

</span>
