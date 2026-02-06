# Pre-Trade Risk Control

The **RiskManager** module is a functional module used for **pre-trade risk control**. Users can conveniently complete risk-control tasks by operating through its JSON configuration file.

## Key Advantages

The RiskManager module provides standardized development templates for risk-control rules and supports users in developing various custom risk-control rules as needed.

Built-in common risk-control rules include:

* **BlackListRule**: blacklist rule
* **WhiteListRule**: whitelist rule
* **OrderLimitRule**: order quantity and value limit rule
* **SelfTradeRule**: self-trade restriction rule
* **RiskLevelRule**: account risk-level limit rule
* **OrderFlowRule**: order flow / rate limit rule
* **PriceRangeRule**: price deviation rule
* **PosLimitRule**: position upper-limit rule
* **TradeValueRule**: intraday opening exposure limit rule

## Starting the Module

Before starting, the RiskManager module must be loaded via the **[Strategy Application]** tab.

After launching **VeighNa Elite Trader**, **connect the trading gateway/interface first** before starting the module. Only start the module after the **[Log]** panel in the main UI outputs: **“Contract information query succeeded”**.

Note: For the **IB gateway**, since it cannot automatically fetch all contract information at login, contract information is only available after the user manually subscribes to market data. Therefore, you must **manually subscribe to the contract’s market data in the main UI first**, then start the module.

After successfully connecting the trading interface, click **[Functions] -> [Risk Control Engine]** in the menu bar, or click the icon in the left-side toolbar:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/riskmanager/1.png)

This will open the UI of the risk control engine module, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/riskmanager/2.png)

## Configuring Risk Control

The pre-trade risk-control module checks whether an order complies with various risk-control rules **before** the order is sent via the trading API.

Users can configure risk-control rules by editing `risk_engine_setting.json` under the `.vntrader` folder, as shown below (example only):

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/riskmanager/3.png)

**Important:** When configuring a risk-control rule, the rule’s `active` field must be set to `true` to enable it. If set to `false`, the rule will not be enabled.

After configuring the risk-control rules, start **VeighNa Elite Trader** and load the risk-control engine module. Then, before each order is sent, the system will check whether it meets the risk-control requirements. If an order is blocked by the risk-control engine, the main UI **[Log]** panel will output the relevant log, and the risk-control engine UI will also output a log, as shown below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/riskmanager/4.png)

## Risk-Control Rule Descriptions

### BlackListRule: Blacklist Rule

* `black_list [list[str]]`: blacklist

After enabled, orders whose `vt_symbol` is in the blacklist will be blocked.

### WhiteListRule: Whitelist Rule

* `white_list [list[str]]`: whitelist

After enabled, orders whose `vt_symbol` is **not** in the whitelist will be blocked.

### OrderLimitRule: Order Quantity and Value Limit Rule

* `order_cancel_limit [int]`: maximum number of cancellations per day
* `order_volume_limit [int]`: maximum volume per single order
* `order_value_limit [float]`: maximum value per single order

After enabled:

* Orders for which the contract information for the corresponding `vt_symbol` cannot be found will be blocked.
* Orders will be blocked if daily cancellations exceed the configured limit.
* Orders will be blocked if single-order volume exceeds the configured limit.
* Orders will be blocked if single-order value exceeds the configured limit.

### SelfTradeRule: Self-Trade Restriction Rule

After enabled, an order will be blocked if:

* Its direction is opposite to an existing unfilled order **and**
* Its price exceeds the price of the unfilled order.

(Example for a long position: the new order is **buy**, the existing unfilled order is **sell**, and the new order price is **greater than or equal to** the sell order’s price.)

### RiskLevelRule: Account Risk-Level Limit Rule

* `risk_level_limit [float]`: maximum margin risk level (single account only)

After enabled:

* Orders sent when the gateway cannot retrieve the account’s current funds will be blocked.
* Orders will also be blocked when **frozen funds / account balance** is **below** the configured margin risk-level threshold.

### PriceRangeRule: Price Deviation Rule

* `price_range_limit [float]`: allowed price deviation

After enabled:

* Orders will be blocked if real-time market data for the corresponding `vt_symbol` is not available.
* Orders will be blocked if the order price exceeds the contract’s limit-up/limit-down prices.
* Orders will be blocked if the order price is outside the allowed upper/lower bounds.

Note: The upper/lower bounds are computed based on:
`latest_price * (1 +/- price_range_limit)`
Then `min/max` is applied, and the result is adjusted according to the contract’s price tick size.

### PosLimitRule: Position Upper-Limit Rule

* `contract_setting [dict]`

  * `vt_symbol` (key)
  * `setting` (value)

    * `long_pos_limit [int]`: long position limit
    * `short_pos_limit [int]`: short position limit
    * `net_pos_limit [int]`: net position limit
    * `total_pos_limit [int]`: total position limit
    * `oi_percent_limit [float]`: intraday net traded value limit

After enabled:

* Orders will be blocked if real-time market data for the corresponding `vt_symbol` is not available.
* Orders will be blocked if the contract’s total long position, total short position, net position, total position, or position concentration exceeds limits.

Note: Position concentration is considered exceeded when the contract’s total position is greater than:
`tick.open_interest * oi_percent_limit`.

### TradeValueRule: Intraday Opening Exposure Limit Rule

* `contract_setting [dict]`

  * `vt_symbol` (key)
  * `setting` (value)

    * `trade_value_limit [int]`: upper limit on intraday change in traded exposure

After enabled, if the exposure impact of a new order plus the cached intraday exposure for that contract exceeds the configured limit, the order will be blocked.

Note: Each filled order’s exposure is calculated as:
`order_price * order_volume * contract_multiplier`.

### OrderFlowRule: Order Flow / Rate Limit Rule

* `order_flow_interval [int]`: time window for order flow control
* `order_flow_limit [int]`: max number of orders allowed within the time window
* `total_order_limit [int]`: max total number of orders per day

After enabled:

* Orders will be blocked if the number of orders sent within the configured time window exceeds the maximum allowed.
* Orders will also be blocked if the number of orders sent exceeds the configured daily total order limit.

Note: The daily total order count depends on the length of all orders found by the main engine.
