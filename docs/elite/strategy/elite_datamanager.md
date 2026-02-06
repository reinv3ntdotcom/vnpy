# Historical Data Management

DataManager is a functional module for **historical data management**. Users can conveniently complete tasks such as data download, data viewing, data import, data export, and data update through its UI interface operations.

## Main Advantages

The DataManager module not only provides support for data download, data viewing, data import, data export, and data update, but also offers options data update functionality to enable stronger options strategy support.

## Starting the Module

The DataManager module needs to be loaded through the [Strategy Application] tab before starting.

After starting and logging into VeighNa Elite Trader, click [Functions] -> [Historical Data Management] in the menu bar, or click the icon in the left button bar:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/1.png)

This will enter the historical data management UI interface, as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/2.png)

## Downloading Data

The DataManager module provides a one-click function to download historical data. Click the [Download Data] button at the top of the interface to pop up the download historical data window, as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/3.png)

You need to fill in four fields: code, exchange, interval, and start date:

<span id="jump">

- Code
  - The code format is the contract symbol
  - Such as IF2412, rb2406
- Exchange
  - The exchange where the contract is traded (click the arrow button on the right side of the window to select from the list of exchanges supported by VeighNa)
- Interval
  - MINUTE (1-minute K-line)
  - HOUR (1-hour K-line)
  - DAILY (daily K-line)
  - WEEKLY (weekly K-line)
  - TICK (one Tick)
- Start Date
  - Format is yy/mm/dd
  - Such as 2021/6/6

</span>

After filling in, click the [Download] button below to start the download program. A successful download is shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/4.png)

Note that the historical data after download completion will be saved in the local database, and can be directly used for subsequent backtesting or live trading without needing to download repeatedly each time.

### Data Source: Data Service (Futures, Stocks, Options)

Taking Xuntou Research as an example, [Xuntou Research](https://xuntou.net/#/signup?utm_source=vnpy) provides historical data for domestic futures, stocks, and options. Before use, ensure that the data service is correctly configured (for configuration details, see the Global Configuration section in the Basic Usage chapter).

### Data Source: IB (Foreign Futures, Stocks, Spot, etc.)

Interactive Brokers (IB) provides rich historical data downloads for foreign markets (including stocks, futures, options, spot, etc.). Note that before downloading, you need to start the IB TWS trading software, connect to the IB interface in the VeighNa Elite Trader main interface, and subscribe to the required contract quotes.

## Importing Data

If you have already obtained data files in CSV format from other channels, you can quickly import them into the VeighNa database using the DataManager's data import function. Click the [Import Data] button in the upper right corner to pop up the dialog box as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/5.png)

Click the [Select File] button at the top to pop up a window to select the path of the CSV file to import, as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/6.png)

Then configure the details related to data import:

- Contract Information
  - For format details, see the introduction in the [Downloading Data](#jump) section of this chapter;
  - Please note that the combination of the imported contract code (symbol) and exchange (exchange) fields forms the local code (vt_symbol) used in modules such as CTA backtesting;
  - If the contract code is **rb2406** and the exchange is selected as **SHFE** (Shanghai Futures Exchange), then the local code to be used for backtesting in CtaBacktester should be **rb2406.SHFE**;
  - You can select the timestamp time zone;
- Header Information
  - You can view the header information of the CSV file and input the corresponding header strings in the header information;
  - For fields that do not exist in the CSV file (such as stock data without the [Open Interest] field), leave them blank;
- Format Information
  - Use the time format definition from Python's built-in datetime module to parse the timestamp string;
  - The default time format is "%Y-%m-%d %H:%M:%S", corresponding to "2024-1-3 0:00:00";
  - If the timestamp is "2024-1-3  0:00", then the time format should be "%Y-%m-%d %H:%M".

After filling in, it will look as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/7.png)

Click the [OK] button to start importing data from the CSV file into the database. During the import process, the interface may appear semi-frozen; the larger the CSV file (the more data), the longer the freeze time. After successful loading, a window will pop up displaying the successful load, as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/8.png)

## Viewing Data

There are currently three ways to obtain data in VeighNa Elite Trader:

- Download via data service or trading interface

- Import from CSV file

- Record using the DataRecorder module

Regardless of the method used to obtain data, click the [Refresh] button in the upper left corner to see the statistical status of the existing data in the current database (excluding Tick data). During the refresh, the interface may experience occasional lag; generally, the more data, the longer the lag time. After a successful refresh, it is shown as in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/9.png)

Click the [View] button to pop up a dialog box for selecting the data interval to view (you can view based on exchange for each time frequency), as shown in the figures below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/12.png)

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/11.png)

After selecting the data range and exchange to display, click the [OK] button to see the specific data fields at each time point in the table on the right:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/13.png)

With existing data in the database, click the small arrow before the data frequency under the [Data] column on the far left of the table to expand or collapse the contract information display under that data frequency.

If the right area of the table is not fully displayed, you can drag the horizontal scroll bar at the bottom of the interface to adjust.

## Exporting Data

If you want to export data from the database to a local CSV file, select the contract to export, click the [Export] button on the right side of the row where the contract is located, and a dialog box for selecting the data interval will pop up, as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/14.png)

Select the data interval range to export, click [OK], and another dialog box will pop up to select the location of the output file, as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/15.png)

After selecting the directory where the exported file will be placed and filling in the CSV file name, click the [Save] button to complete the export of the CSV file.

## Deleting Data

If you want to delete data for a specific contract, select the contract to delete, click the [Delete] button on the right side of the contract row data, and a dialog box will pop up, as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/16.png)

Click the [OK] button to delete the contract data, and a window will pop up showing successful deletion, as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/17.png)

At this point, click the [Refresh] button again, and the graphical interface will no longer show information for that contract, as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/18.png)

## Updating Data

When the user has **configured the data service** or **the trading interface (connected) provides sufficient historical data**, click the [Update Data] button at the top of the interface to perform a one-click automatic download update based on all contract data displayed on the graphical interface.

The graphical interface display before updating is as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/19.png)

Click the [Update Data] button, and an information prompt dialog box for update progress will pop up. At this time, DataManager will automatically **download data from the end date of the existing data in the database up to the current latest date** and update it to the database.

If the data to be updated is small, the update task may be completed instantly, and it is normal not to observe the update dialog box.

After the update is completed, click the [Refresh] button in the upper left corner to see that the contract data has been updated to the current latest date.

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/20.png)

## Data Range

Please note that although the interface displays the start and end times of the existing data in the database, **it does not mean that the database stores all data from the start time to the end time**.

If relying on historical data provided by the trading interface, once the time span between the start time and end time exceeds the data range that the interface can provide, it may lead to missing data in between. Therefore, it is recommended to click the [View] button after updating the data to check if the contract data is continuous.

## Updating Options Data

After configuring the [Exchange] and [Start Time] in the upper right corner of the interface, click the [Update Options Data] button to update the options contract information and historical data stored in the database based on the exchange, as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/21.png)

After the update is completed, click the [Refresh] button in the upper left corner to see that the options contract data has been updated to the current latest date.

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/22.png)

If you want to stop the data update during the update process, you can click the [Stop Data Update] button in the upper right corner to stop the update task, as shown in the figure below:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/datamanager/23.png)

Please note:

 - Currently, the options data update function can only be used if Xuntou Research or RiceQuant is configured
 - The above image is a screenshot of updating with Xuntou Research. If using RiceQuant as the data service, the output "Starting to update contract historical information" will not appear during the update process
 - If you click the close button in the upper right corner of the graphical interface during the update process to close the historical data management UI interface, the update task will stop
