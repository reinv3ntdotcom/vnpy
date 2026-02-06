## Portfolio Strategy Cross-Section Strategy

**PortfolioStrategy** is a functional module used for **live multi-process portfolio strategies**. Through its UI, users can conveniently complete tasks such as strategy initialization, strategy start, strategy stop, strategy parameter editing, and strategy removal.

---

## Main Advantages

The PortfolioStrategy module makes full use of multi-core CPUs and supports multi-process portfolio strategy trading.

---

## Starting the Module

Before starting, the PortfolioStrategy module must be loaded via the **[Strategy Apps]** tab.

After logging into **VeighNa Elite Trader**, before starting the module, please connect to the trading gateway first. Only start the module after the **[Log]** panel in the main window outputs **“Contract information query succeeded”**.

Note: For the **IB** gateway, because it cannot automatically retrieve all contract information at login, it can only obtain contract info after the user manually subscribes to market data. Therefore, you must first manually subscribe to the contract’s market data in the main window before starting the module.

After successfully connecting to the trading gateway, click **[Functions] -> [Multi-process Portfolio Strategy Trading]** in the menu bar, or click the icon in the left-side toolbar:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/1.png)

This will open the UI of the multi-process portfolio strategy trading module.

If a data service is configured, the module will automatically perform data-service login initialization when opened. If login succeeds, the log will output **“Data service initialization succeeded”**, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/2.png)

---

## Strategy File Directory

<span id="jump">

For strategies developed by users, they must be placed in the **strategies** directory under the VeighNa Elite Trader runtime directory to be detected and loaded. You can view the exact runtime directory path in the title bar at the top of the VeighNa Elite Trader main window.

For users on Windows with the default installation, the strategies directory path is typically:

```text
C:\Users\Administrator\strategies
```

Where `Administrator` is the currently logged-in Windows username.

</span>

---

## Creating a Strategy Instance

Users can create different strategy instances (objects) based on a written portfolio strategy template (class).

Select the strategy name to trade from the drop-down box in the upper-left corner, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/3.png)

Note: The displayed strategy name is the name of the **strategy class** (CamelCase), not the strategy file name (snake_case).

After selecting the strategy class, click **[Add Strategy]** to open the “Add Strategy” dialog:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/4.png)

When creating a strategy instance, you need to configure parameters as follows:

* **Instance name**

  * Must be unique (no duplicates).
* **Contract symbols**

  * Format: `vt_symbol` (contract code + exchange name).
  * Must be a contract name that can be found in the live trading system.
  * Separate multiple contracts with “,” and **do not** add spaces.
* **Gateway name**

  * Select the gateway to trade through.
* **Parameter settings**

  * Parameter names come from the strategy’s `parameters` list.
  * Default values are the strategy’s default parameter values.
  * As seen in the figure, the angle brackets `<>` after a parameter name show its data type. When entering values, follow the correct type: `<class 'str'>` = string, `<class 'int'>` = integer, `<class 'float'>` = floating-point.
  * Important: If a parameter may later need decimal values but its default is an integer (e.g., `1`), then in the strategy code you should set the default as a float (e.g., `1.0`). Otherwise the UI will treat it as an integer and only allow integers when later editing the parameter.

After completing the configuration, click **[Add]** to create the strategy instance. If successful, you’ll see the instance in the strategy monitor panel on the left. Since each strategy runs in an independent process, the UI will output a log like **“Strategy process started”**, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/5.png)

At the top of the monitor component it shows the instance name, gateway name, strategy class name, and strategy author name (`author` defined in the strategy). The top buttons are used to control/manage the instance.
The first table row shows the strategy’s parameter info (parameters must be listed in `parameters` to display).
The second row shows runtime variables (variables must be listed in `variables` to display).
`inited` indicates whether initialization has completed (i.e., historical data replay finished).
`trading` indicates whether the strategy is currently allowed to trade.

From the figure, both `inited` and `trading` are **False**, meaning the instance has not been initialized and cannot send trading signals.

After creation, the configuration is saved in:
`.vntrader/portfolio_strategy_setting.json`.

---

## Initializing a Strategy

After creating an instance, you can initialize it. Click **[Init]** under the instance. If initialization succeeds:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/6.png)

After initialization, `inited` becomes **True** (historical data loaded and initialization completed). `trading` remains **False**, so the strategy still cannot auto-trade yet.

---

## Starting a Strategy

Only when initialization succeeded (`inited` = **True**) can you start auto-trading. Click **[Start]** under the instance. If successful:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/7.png)

Now both `inited` and `trading` are **True**, meaning historical replay is complete and the strategy’s order-request functions (`buy/sell/short/cover/cancel_order`, etc.) and notification functions (`send_email/put_event`, etc.) will actually execute and send real requests to the underlying gateway (real trading).

During initialization, even though the strategy receives (historical) data and calls functions, because `trading` is **False**, no real orders are placed and no trade-related logs are output.

If the strategy sends orders after starting, you can view order details in the VeighNa Trader main window under **[Orders]**.

Note: Unlike the CTA strategy module, multi-contract portfolio strategies **do not provide local stop orders**, so there is no stop-order display area in the UI.

---

## Stopping a Strategy

After starting, if you need to stop, edit, or remove a strategy (e.g., market close or emergency), click **[Stop]** under the instance. If successful:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/8.png)

The portfolio strategy engine will automatically cancel all active orders previously sent by the strategy to ensure no unmanaged orders remain. The latest variable state is saved to:
`.vntrader/portfolio_strategy_data.json`.

At this point, `trading` becomes **False**, meaning auto-trading is stopped.

In live trading, normally you should let strategies run automatically throughout the trading session and avoid extra pause/restart operations. For China’s domestic futures market, you should start auto-trading before the session begins and stop it after the market closes. Since the CTP night session close also shuts down the system and restarts before the morning session, you also need to stop the strategy and close VeighNa Trader after the night session ends.

---

## Editing a Strategy

If you want to edit an instance’s parameters after creation (if it’s running, stop it first), click **[Edit]** under the instance to open the parameter edit dialog:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/9.png)

After editing, click **[OK]** and the changes will immediately update in the parameter table:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/10.png)

However, the traded contract codes cannot be changed, and editing does not rerun initialization. Also note: this only updates the instance parameters stored in `.vntrader/portfolio_strategy_setting.json`, not the original strategy file’s defaults.

To start again after editing, click **[Start]**:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/11.png)

---

## Removing a Strategy

To remove a strategy instance (stop it first if running), click **[Remove]**. After removal, it no longer appears in the left monitoring panel. Because each strategy is a separate process, the UI will output a log like **“Strategy process exited”**:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/12.png)

The instance configuration is also removed from `.vntrader/portfolio_strategy_setting.json`.

---

## Status Tracking

There are two ways to track strategy status via the UI:

1. **Call `put_event`**

   * All variable info must be listed in the strategy’s `variables` list to be displayed.
   * To refresh variable changes on the UI, the strategy must call `put_event`.
   * If variables never change in the UI, check whether you forgot to call `put_event`.

2. **Call `write_log`**

   * If you want to see variable changes and also output custom logs, call `write_log` in the strategy.

---

## Runtime Logs

### Log Content

Logs in the multi-contract portfolio strategy UI come from two sources: the strategy engine and strategy instances.

**Engine logs**
The engine typically outputs global info. In the figure below, everything except the content after the instance name + colon is engine output:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/13.png)

**Strategy logs**
If the strategy calls `write_log`, the content is shown as strategy logs. In the red box below, the content is strategy-instance output. Before the colon is the instance name; after the colon is the `write_log` output:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/14.png)

### Clear Logs

To clear the log output on the UI, click **[Clear Logs]** in the upper-right corner.

Before clearing:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/12.png)

After clearing:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/15.png)

---

## Batch Operations

After strategies are fully tested and stable in live trading (not frequently adjusted), if you have multiple portfolio strategy instances to run, you can use **[Init All]**, **[Start All]**, and **[Stop All]** in the upper-right corner to perform batch initialization before market open, batch start, and batch stop after market close.

---

## Multi-Account Support

### Loading

The multi-process portfolio strategy module supports multi-account batch order placement.

Using **CTP** login as an example: in the login window under the **[Trading Gateway]** tab, select the CTP gateway from the dropdown. Enter a custom gateway name (e.g., “CTP1”, “CTP2”) under “Custom Gateway” and click **[Add]**. Fill in the sub-account configuration and click **[OK]**. Repeat to load multiple account gateways.

After adding, click **[Login]** to log into VeighNa Elite Trader. Then in the menu bar click **[System] -> [Connect xxx]** (where `xxx` is the custom gateway name; if you used “CTP1”, the menu will show **[Connect CTP1]**) to connect each sub-account gateway.

After successful connection, the main **[Log]** component will output login information, and you can also see account info, positions, etc.

### Batch Ordering via the Multi-Process Portfolio Strategy Module

To place batch orders via the module, when you click **[Add Strategy]**, select the desired gateway in the **gateway_name** dropdown:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/16.png)

After adding successfully, the left monitoring component will show the strategy instance info:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/17.png)

After the strategy sends orders, you can track them in the VeighNa Elite Trader main window under **[Orders]** and **[Trades]**:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/portfoliostrategy/18.png)

**Note:**

* Currently supports logging into up to **5** trading accounts at the same time.

---

## Multi-Contract Portfolio Strategy Template

The multi-contract portfolio strategy template provides signal generation and order management functions. Users can develop multi-contract portfolio strategies based on this template.

User-developed strategies can be placed in the user runtime folder’s **[strategies](#jump)** directory.

Note:

* Strategy filenames use snake_case, e.g. `portfolio_boll_channel_strategy.py`, while strategy class names use CamelCase, e.g. `PortfolioBollChannelStrategy`.
* Do not name your custom strategy class the same as an example strategy class. If they overlap, only one class name will show in the UI.

### StrategyTemplate

VeighNa Elite Trader supports the built-in `StrategyTemplate` from `vnpy_portfoliostrategy`. Strategies developed with `StrategyTemplate` can run successfully in VeighNa Elite Trader’s multi-contract portfolio strategy module.
