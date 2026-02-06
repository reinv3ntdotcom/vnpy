# Algorithmic Trading Execution
AlgoTrading is a module for **algorithmic order execution trading**. Users can conveniently perform tasks such as starting algorithms, saving configurations, and stopping algorithms through its UI interface.
## Main Advantages
Algorithmic trading handles the specific execution process of order placements. Currently, AlgoTrading provides multiple example algorithms. Users can automatically split large orders into suitable small orders for batch placement, effectively reducing trading costs and impact costs. It can also perform high sell low buy operations within set thresholds. Additionally, the module can monitor CSV files in specified paths for intelligent order sweeping.
## Starting the Module
For user-built algorithms, they need to be placed in the vnpy_algotrading.algos directory to be recognized and loaded.
The AlgoTrading module needs to be loaded via the [Strategy Applications] tab before starting.
After starting and logging into VeighNa Elite Trader, before starting the module, please connect to the trading interface first. Start the module only after seeing the "Contract information query successful" output in the [Log] column of the VeighNa Elite Trader main interface.
Please note that the IB interface cannot automatically obtain all contract information upon login; it can only be obtained when the user manually subscribes to market quotes. Therefore, you need to manually subscribe to contract quotes on the main interface first, then start the module.
After successfully connecting to the trading interface, click [Functions] -> [Multi-Process Algorithmic Trading] in the menu bar, or click the icon in the left button bar:
![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/0.png)
This will enter the UI interface of the multi-process algorithmic trading module, as shown below:
![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/1.png)
## Configuring the Algorithm
Configuration parameter requirements are as follows:
- Algorithm: Select the trading algorithm to execute from the dropdown box;
- Exchange: Select the exchange from the dropdown box;
- Code: Format is symbol (contract code);
- Direction: Long, Short;
- Offset: Open, Close, Close Today, Close Yesterday;
- Price: The price for placing the order;
- Order Quantity: The total quantity of the order;
- Interface: Select the interface to send the order from the dropdown box.
## Starting the Algorithm
Currently, VeighNa provides five common example algorithms. This document uses the Time-Weighted Average Price algorithm (TWAP) as an example to introduce the algorithm starting process.
After filling in the algorithm configuration in the upper left corner of the graphical interface, click the [Start Algorithm] button to start the algorithm, as shown below:
![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/3.png)
If started successfully, the execution status of the algorithm can be observed in the upper right [Executing] interface. And in the lower right [Log] interface, you will see the "Algorithm started" log output.
The specific task executed by the algorithm in the figure is: Using the time-weighted average algorithm, buy 20 lots of CSI 300 stock index futures 2409 contract (IF2409), execution price is 3422 yuan, execution time is 120 seconds, interval per round is 6 seconds; that is, every 6 seconds, when the contract's ask price 1 is less than or equal to 3422, buy 1 lot of CSI 300 stock index futures 2409 contract at the price of 3422, splitting the buy operation into 20 times.
## CSV Monitoring
When there are many algorithms to start, you can start them by monitoring CSV files. Click the [CSV Monitoring] button on the left side of the graphical interface, and in the pop-up dialog, select the folder to monitor for newly written CSV files under the path, as shown below:
![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/7.png)
![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/8.png)
Please note that the format of the CSV file should be as shown below, consistent with the fields in the left editing area:
![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/9.png)
After successful startup, the execution status of all algorithms in the CSV file will be displayed in the [Executing], [Completed], and [Log] interfaces, as shown below:
![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/10.png)
## Pausing the Algorithm
When the user needs to pause an executing trading algorithm, click the [Pause] button in the [Executing] interface to pause a specific executing algorithmic trade, as shown below:
![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/4.png)
## Resuming the Algorithm
When the user needs to resume a paused trading algorithm, click the [Resume] button in the [Executing] interface to resume a paused algorithmic trade, as shown below:
![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/algotrading/5.png)
## Stopping the Algorithm
When the user needs to stop an executing trading algorithm, click the [Stop] button in the [Executing] interface to stop a specific executing algorithmic trade, as shown below:
![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/6.png)
The user can also click the [Stop All] button at the bottom of the order trading interface to stop all executing algorithmic trades with one click.
## Data Monitoring
The data monitoring interface consists of three parts:
Executing Component: Displays executing algorithmic trades, including: algorithm, parameters, and status. After successfully starting the algorithm, switch to the upper right [Executing] interface to display the execution status of the algorithm.
Completed Component: Displays completed algorithmic trades, also including: algorithm, parameters, and status. After the algorithm ends or stops, switch to the upper right [Completed] interface to display the execution status of the algorithm.
Log Component: Displays related log information for starting, stopping, and completing algorithms.
## Example Algorithms
The example algorithms are located in the vnpy_algotrading.algos folder (please note that some algorithms do not have offset directions written; if needed, they can be customized based on individual requirements). Currently, the algorithmic trading module provides the following five built-in algorithms:
### TWAP - Time-Weighted Average Price Algorithm
The Time-Weighted Average Price algorithm (TWAP) specific execution steps are as follows:
- Distribute the order quantity evenly over a certain time period, placing buy orders (or sell orders) at specified prices at regular intervals.
- Buy scenario: When the ask price 1 is below the target price, place an order, with the order quantity being the minimum of the remaining order quantity and the split order quantity.
- Sell scenario: When the bid price 1 is above the target price, place an order, with the order quantity being the minimum of the remaining order quantity and the split order quantity.
### Iceberg - Iceberg Algorithm
The Iceberg algorithm (Iceberg) specific execution steps are as follows:
- Place orders at a certain price level, but only expose a portion until fully executed.
- Buy scenario: First check for cancellations, if the latest Tick ask price 1 is below the target price, execute cancellation; if no active orders, place an order, with the order quantity being the minimum of the remaining order quantity and the exposed order quantity.
- Sell scenario: First check for cancellations, if the latest Tick bid price 1 is above the target price, execute cancellation; if no active orders, place an order, with the order quantity being the minimum of the remaining order quantity and the exposed order quantity.
### Sniper - Sniper Algorithm
The Sniper algorithm (Sniper) specific execution steps are as follows:
- Monitor the market quotes pushed by the latest Tick, and immediately quote to execute when a good price is found.
- Buy scenario: When the latest Tick ask price 1 is below the target price, place an order, with the order quantity being the minimum of the remaining order quantity and the ask quantity 1.
- Sell scenario: When the latest Tick bid price 1 is above the target price, place an order, with the order quantity being the minimum of the remaining order quantity and the bid quantity 1.
### Stop - Conditional Order Algorithm
The Conditional Order algorithm (Stop) specific execution steps are as follows:
- Monitor the market quotes pushed by the latest Tick, and immediately quote to execute when the market breaks through.
- Buy scenario: When the Tick latest price is above the target price, place an order, with the order price being the target price plus the slippage.
- Sell scenario: When the Tick latest price is below the target price, place an order, with the order price being the target price minus the slippage.
### BestLimit - Best Limit Algorithm
The Best Limit algorithm (BestLimit) specific execution steps are as follows:
- Monitor the market quotes pushed by the latest Tick, and immediately quote to execute when a good price is found.
- Buy scenario: First check for cancellations: if the latest Tick bid price 1 is not equal to the target price, execute cancellation; if no active orders, place an order, with the order price being the latest Tick bid price 1, and the order quantity being the remaining order quantity.
- Sell scenario: First check for cancellations: if the latest Tick bid price 1 is not equal to the target price, execute cancellation; if no active orders, place an order, with the order price being the latest Tick ask price 1, and the order quantity being the remaining order quantity.
