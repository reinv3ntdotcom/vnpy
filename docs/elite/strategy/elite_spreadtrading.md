# Spread Arbitrage Strategy

**SpreadTrading** is a functional module for **multi-contract spread arbitrage**. Through its UI, users can conveniently create flexible spread contracts and perform tasks such as semi-automatic algorithmic trading and fully automated strategy trading.

## Key Advantages

The SpreadTrading module supports three spread execution algorithms—**Taker**, **Maker**, and **Exchange**—and also provides a professional strategy template, **EliteSpreadStrategyTemplate**, enabling more powerful spread strategy development.

## Starting the Module

The SpreadTrading module must be loaded from the **[Strategy Apps]** tab before it can be started.

After logging into **VeighNa Elite Trader**, before starting the module, you must first connect the trading gateway. Only start the module after the **[Log]** panel in the main window shows **“Contract information query succeeded”** (**if you open the module before the contract info query succeeds, the spread tick “price jump” may be read as zero, which can trigger low-level errors after orders are filled**).

Note: the **IB gateway** cannot automatically obtain all contract information at login—contracts are only retrieved when the user manually subscribes to market data. Therefore, you must manually subscribe to the contract quotes in the main window first, then start the module.

After successfully connecting the trading gateway, click **[Functions] → [Spread Arbitrage Trading]** on the menu bar, or click the icon on the left-side toolbar to enter the Spread Arbitrage Trading UI.

## Strategy File Directory

<span id="jump">

Custom strategies developed by users must be placed in the **strategies** directory under the VeighNa Elite Trader runtime directory in order to be recognized and loaded. You can see the runtime directory path in the title bar at the top of the VeighNa Elite Trader main window.

For a default installation on Windows, the strategies directory is typically:

```text
C:\Users\Administrator\strategies
```

Where `Administrator` is the current Windows username.

</span>

## Creating a Spread Contract

### Querying Contracts

Before creating a spread contract, users can use **[Query Contract]** to find contracts that can form a spread (**exchange spread arbitrage contracts are not supported**):

* In the VeighNa Elite Trader menu bar, click **[Help] → [Query Contract]** to open the contract query window.
* Find contracts that can be used for spread trading in the window.
* This document uses a soybean oil futures calendar spread as an example: trading **y2309.DCE** (Sep 2023 soybean oil futures) and **y2401.DCE** (Jan 2024 soybean oil futures).

### Building a Spread Contract

On the left side of the spread trading UI, click **[Create Spread]** to open the spread creation window.

The module supports flexible spread formulas (e.g., **A/B**, **A - B*C**, etc.) and also allows “pricing legs” that do not participate in trading—useful for complex onshore/offshore arbitrage spreads that must consider FX rates, taxes, and other factors. When creating a spread contract, configure these parameters:

* **Spread Name**

  * User-defined spread contract name
  * Must be unique (no duplicates)
* **Active Leg Symbol**

  * The local symbol of the leg that is submitted first when spread order-book conditions are met
  * Format: `vt_symbol` (contract symbol + exchange)
  * Must be one of the configured legs below
* **Minimum Volume**

  * Minimum number of lots
* **Price Formula**

  * Calculation formula of the spread contract
  * Supports any built-in Python math functions
  * Variables can only be **A, B, C, D, E** (not all must be used)
* **[A, B, C, D, E]**

  * Legs that form the spread (active/passive) and optional non-trading pricing legs; each is composed of contract code, trading direction, and trading multiplier:

    * Contract code: the `vt_symbol` corresponding to the variable in the formula
    * In principle, after the active leg fills, the passive leg hedges immediately. Therefore:

      * Active leg is typically a less liquid contract; its price multiplier and trade multiplier are usually positive.
      * Passive leg is typically a more liquid contract; its price multiplier and trade multiplier are usually negative (in the GUI choose **Sell**, but keep the trade multiplier as a positive number; in Jupyter backtests, both are passed as negative numbers).
    * Leave unused variables empty.

After setting the parameters, click **[Create Spread]** to create the spread contract.

In the soybean oil calendar spread example, both the price multiplier and trade multiplier are **1:1**, i.e.
**spread = y2401 − y2309**.
Buying 1 lot of the spread equals buying 1 lot of y2401 and selling 1 lot of y2309 to hedge.

Note: with multiple legs and futures contracts of different contract sizes, building a spread becomes more complex. For example, in a “virtual steel mill” arbitrage spread:

* Producing rebar uses **16 tons of iron ore + 5 tons of coke** to make **10 tons of rebar**
* Price-multiplier spread formula: **spread = 1*RB − 1.6*I − 0.5*J**
* But contract sizes differ: RB is **10 tons/lot**, while iron ore and coke are both **100 tons/lot**, so the trade multipliers are **1:10:10**
* Using the greatest common divisor rule, the actual hedging lot relationship is:
  for every **buy 100 lots of RB** (1000 tons), you must **sell 16 lots of I** (1600 tons) and **sell 5 lots of J** (500 tons)

### Monitoring a Spread Contract

After creation, the **[Log]** panel outputs “Spread created successfully”, and the **[Spread]** panel shows real-time market data.

In the soybean oil example, fields mean:

* **Bid Price**

  * y2401 best bid − y2309 best ask
* **Bid Volume**

  * `min(y2401 bid size, y2309 ask size)`
  * Uses the minimum to help ensure both legs can fill
* **Ask Price**

  * y2401 best ask − y2309 best bid
* **Ask Volume**

  * `min(y2401 ask size, y2309 bid size)`

### Removing a Spread Contract

On the left side, click **[Remove Spread]** to open the removal window. Select the spread, then click **[Remove]**. The **[Log]** outputs “Spread removed successfully”.

## Semi-Automatic Algorithmic Trading

In the top-left **[Trading]** component of the module UI, you can choose an algorithm (currently supports **three algorithms**—see the section “Algorithms”) and run semi-automatic algorithmic trading.

When filling parameters, note:

* **Open/Close**: only choose **[Open]** or **[Close]** when using the **Exchange** algorithm. In most other cases use **[Net Position]**; for stock index futures use **[Lock Position]**.

Below are two examples using **SpreadTakerAlgo** (aggressive “cross the spread” execution): one that fills immediately, and one that waits.

### Start and Fill Immediately (Paying Up)

If the target spread price is **-70**, and you start a long algorithm at **-60** (paying up), it submits orders and fills immediately.

### Start and Wait to Fill (Limit)

Start a long algorithm at **-80**. If current bid/ask are **-76 / -72**, the order status shows **[Unfilled]**.

### Stop a Running Algorithm

Double-click the cell for the algorithm you want to stop. The **[Log]** outputs “Algorithm stopped”, and the **[Algo]** panel shows the status changes from **[Unfilled]** to **[Cancelled]**.

## Fully Automated Strategy Trading

### Add a Strategy

Users can create different strategy instances (objects) based on a strategy template class.

Select the strategy name (e.g., **MeanReversionStrategy**) from the dropdown on the left, then click **[Add Strategy]**.

Note: the displayed name is the **strategy class name** (CamelCase), not the file name (snake_case).

In the add-strategy dialog, configure:

* **Instance Name**

  * User-defined strategy instance name; must be unique
* **Spread Name**

  * Spread contract to trade; must be a spread that the Spread component can query
* **Parameter Settings**

  * Parameters are defined in the strategy using the `Parameter` helper class
  * Default values come from the strategy’s defaults
  * The dialog shows each parameter’s type in angle brackets; enter values that match the type (`str`, `int`, `float`)
  * If a parameter might need decimals later, set its default as a float (e.g., `1.0`) in the strategy; otherwise the UI may force it to remain an integer during later edits

Example parameters for **MeanReversionStrategy**:

* `ma_window` (moving average window)
* `entry_range` (entry range)
* `fixed_volume` (fixed trade size)
* `payup` (ticks added relative to the opposing best price when placing orders for each leg)
* `interval` (seconds before cancelling and re-posting an unfilled limit order)

After creation, the strategy instance appears in the strategy monitor panel in the bottom-right.

The strategy monitor shows:

* Instance name, spread name, strategy class name, and author
* A parameter table (must be listed in the strategy’s `parameters` list to display)
* A variable table (must be listed in the strategy’s `variables` list to display)
* `inited`: whether initialization (historical data loading) is complete
* `trading`: whether the strategy is allowed to trade

After creation, the instance config is saved to:
`.vntrader/spread_trading_strategy.json`

### Manage Strategies

#### Initialize

Click **[Initialize]**. If successful, `inited` becomes **True** (historical data loaded via `load_bar`), but `trading` remains **False**.

#### Start

Only when `inited` is **True** can you click **[Start]**. After starting, `trading` becomes **True**. Note: strategy start does not necessarily mean an execution algo is running—algo state depends on strategy logic.

#### Stop

Click **[Stop]** to stop automated trading. `trading` becomes **False**.

When stopping, the engine:

1. stops all algos started by the strategy
2. cancels all unfilled orders
3. then stops strategy trading

If any algos were still running, you will see “Algorithm stopped” in the log and the algo status becomes **Cancelled**.

#### Edit

To edit parameters:

* If the strategy is running, stop it first
* Click **[Edit]** to open the parameter editor, change values, and click **[OK]**

Notes:

* You cannot change the traded contract codes
* Editing does not re-run initialization
* This only modifies the saved config in `spread_trading_strategy.json`, not the original strategy file defaults

#### Remove

To remove an instance:

* Stop it first if running
* Click **[Remove]**

After removal, it disappears from the monitor panel and its entry is deleted from `spread_trading_strategy.json`.

### Batch Operations

If strategies are stable and don’t need frequent adjustments, use:

* **[Initialize All]**, **[Start All]**, **[Stop All]**
  for pre-market initialization, starting strategies, and post-market shutdown.

## Spread Strategy Template

The template provides signal generation and order-management functions; users can develop their own strategies based on it.

Custom strategies should be placed in the user runtime folder’s **strategies** directory (see the Strategy File Directory section).

Notes:

* Strategy filenames use snake_case, e.g. `mean_reversion_strategy.py`
* Strategy class names use CamelCase, e.g. `MeanReversionStrategy`
* Avoid naming your custom strategy class the same as any example strategy class, or they may overwrite each other.

### EliteSpreadStrategyTemplate

The SpreadTrading module provides **EliteSpreadStrategyTemplate**, a professional spread arbitrage strategy template for more powerful development.

Using the MeanReversionStrategy example, strategy development begins by importing internal components at the top of the file:

```python
from vnpy.trader.utility import BarGenerator, ArrayManager

from elite_spreadtrading import (
    SpreadStrategyTemplate,
    SpreadAlgoTemplate,
    AlgoType,
    AlgoOffset,
    Variable,
    Parameter,
    SpreadData,
    OrderData,
    TradeData,
    TickData,
    BarData
)
```

Where:

* `SpreadStrategyTemplate`: spread strategy base template
* `SpreadAlgoTemplate`: spread algo base template
* `AlgoType`, `AlgoOffset`: algo execution mode + open/close option
* `Parameter`: container for strategy parameters
* `Variable`: container for strategy variables
* `SpreadData`, `OrderData`, `TickData`, `TradeData`, `BarData`: data containers
* `BarGenerator`: bar synthesis module
* `ArrayManager`: time series management for bars

### Strategy Parameters and Variables

In the strategy class, set the author, parameters, and variables, e.g.:

```python
author = "A Python trader"

ma_window: int = Parameter(60)
entry_range: int = Parameter(20)
fixed_volume: int = Parameter(10)
payup: int = Parameter(10)
interval: int = Parameter(5)

spread_pos: float = Variable(0.0)
ma_value: float = Variable(0.0)
```

Notes:

* Parameters are fixed inputs specified externally
* Variables change during execution; initialize them with base values (e.g., `0`, `0.0`)
* `Parameter` and `Variable` only accept: `str`, `int`, `float`, `bool`

### Strategy Callback Functions

Functions in `SpreadStrategyTemplate` starting with `on_` are callbacks used to receive spread data and status updates. They’re called automatically by the engine when events occur.

They fall into three categories:

#### Strategy Instance Lifecycle (Required)

**on_init**

* No args / no return
* Typically:

  1. Create `BarGenerator` to build 1-minute bars from ticks (or longer periods if needed)
  2. Create `ArrayManager` for vectorized indicator calculations (supports `talib`)

     * Default length 100; adjust with `size` (must be ≥ indicator period length)
  3. Configure algo mode:

     * `AlgoType`: `TAKER`, `MAKER`, `EXCHANGE`
     * `AlgoOffset`: `LOCK`, `NET`, `OPEN`, `CLOSE`
  4. Call `load_bar` to load history

Example:

```python
def on_init(self):
    self.bg = BarGenerator(self.on_spread_bar)
    self.am = ArrayManager(self.ma_window + 10)

    self.algo_type: AlgoType = AlgoType.TAKER
    self.algo_offset: AlgoOffset = AlgoOffset.NET

    self.write_log("Strategy initialized")
    self.load_bar(10)
```

If backtesting with tick data, call `load_tick` here.

During init, `inited` and `trading` start as False. After `on_init`, `inited` becomes True.

**on_start**

* Writes a “Strategy started” log by default
* After start, `trading` becomes True and algos may run depending on logic

**on_stop**

* Writes a “Strategy stopped” log and calls `put_event()`
* After stop, `trading` becomes False and algos cannot be started

#### Receive Data, Compute Indicators, Generate Signals

**on_spread_data**

* Called when spread data updates
* Example pattern: get spread tick and forward to `on_spread_tick`

**on_spread_tick(tick)**

* Commonly pushes tick into `BarGenerator` via `update_tick` to build bars

**on_spread_bar(bar)**

* Called when a 1-minute bar is formed
* Example approach:

  * cancel stale algos/orders via `stop_all_algos()`
  * update `ArrayManager`, ensure initialized
  * compute indicator (e.g., SMA)
  * start long/short algos based on rules
  * call `put_event()`

#### Order/Position Status Updates

Mostly can be left as `pass` unless custom behavior is needed:

* **on_spread_pos**: reads account-level spread position (default: `self.get_spread_pos()`)
* **on_spread_algo**: algo status update
* **on_order**: order update (only for orders sent directly by the strategy)
* **on_trade**: trade update (only for orders sent directly by the strategy)

### Active (Command) Functions

* **start_long_algo(...) → algoid**
* **start_short_algo(...) → algoid**
* **start_algo(...) → algoid**

  * Only works after the strategy is started (`trading` True). Otherwise returns empty.
* **stop_algo(algoid)**
* **stop_all_algos()**

Also available for sending single-contract orders (not spread orders):

* **buy / sell / short / cover** (each takes `vt_symbol`, `price`, `volume`, `lock=False`)
* Under the hood, they call **send_order(...)**

  * Only works when `trading` is True; otherwise returns an empty list

Cancellation helpers:

* **cancel_order(vt_orderid)**
* **cancel_all()**

### Utility Functions

* **put_event()**: refresh UI display (only after `inited` True)
* **write_log(msg)**: write log messages
* **get_engine_type()**
* **get_spread_tick()**
* **get_spread_pos()**
* **get_leg_tick(vt_symbol)**
* **get_leg_pos(vt_symbol, direction=Direction.NET)**
* **send_email(msg)**: send email after configuration (only after `inited` True)
* **load_bar(days, interval=Interval.MINUTE, callback=None)**: load historical spread bars for init

  * Default example loads 10 days of 1-minute bars; engine tries gateway → data service → DB
  * If any leg has missing bar data for a time segment, that entire segment is discarded for all legs
* **load_tick(days)**: load recorded spread ticks from DB

  * Requires recording via DataRecorder after creating the spread
  * Local symbol format: `xx-spread.LOCAL` (where `xx-spread` is the user-defined spread name)

## Algorithms

<span id="jump1">

Spread arbitrage is executed via spread algos, which simplify complex multi-leg execution into an ordinary spread order by encapsulating the active-leg order placement and passive-leg hedging details.

### Algo Check Functions

* `is_active`: check whether the algo has ended
* `is_order_finished`: check whether all orders are finished
* `is_hedge_finished`: check whether passive and active legs are matched
* `check_algo_cancelled`: check whether the algo was stopped
* `calculate_traded_volume`: compute traded spread volume
* `calculate_traded_price`: compute average traded spread price

### SpreadTakerAlgo (Aggressive “Cross the Spread”)

**How it works**

* Active leg submits at the opposing best price; passive leg hedges immediately at the opposing best price
* On tick: check order completion → check hedge completion → hedge if needed → if conditions met, place active-leg order
* On order update: if active-leg order completes, hedge passive leg
* Timeout: cancel all orders when timer expires

**Pros**

* Flexible and does not consume too many cancel attempts

**Cons**

* All legs pay the bid/ask spread slippage cost
* Waiting for active-leg opposing quote conditions can take longer than Maker

### SpreadMakerAlgo (Quoted “Market-Making”)

**How it works**

* Based on passive-leg order book to compute worst acceptable fill for the active leg
* On tick: check order completion → check hedge completion → hedge if needed → compare desired new quote vs current quote; if change exceeds threshold, re-quote, otherwise keep/submit
* On order update: if rejected, stop strategy; if active-leg completes, clear quote record
* On trade: focus on active-leg fills; hedge passive leg if needed
* Timeout: cancel all orders when timer expires

**Pros**

* Active leg posts quotes aiming to capture spread and increase fill probability

**Cons**

* More frequent cancel/repost than Taker; must monitor message-rate/flow fees carefully

### SpreadExchangeAlgo (Exchange-Provided Spread Contract)

**How it works**

* Creates a spread based on an exchange-provided spread instrument
* Uses the legs’ order books to compute spread quotes, but execution is done via the exchange’s spread contract
* On tick: if already sent an order, return; otherwise ensure spread contract is created, query its contract info, send exchange spread order, cache order ID and spread mapping, log, and mark as sent

**Pros**

* Feels like single-instrument trading and avoids active-leg cancellations

**Cons**

* Less flexible; limited contract choices

</span>
