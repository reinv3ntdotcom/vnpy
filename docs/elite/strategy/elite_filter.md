# Market Data Filtering

The CTA strategy module of VeighNa Elite Trader has built-in support for filtering junk data in the EliteCtaTemplate. After configuring according to the example format, EliteCtaTemplate will filter synthesized K-lines received during non-trading periods, avoiding the impact of junk data on the calculation results of strategy indicators.

## Configuring Filter Information

### Using the Officially Provided File

In the VeighNa Elite Trader main interface, click [Help] - [Update Tick Filter] to generate the latest filter configuration file filter_setting.json, as shown in the following images:

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/filter/1.png)

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/elite/filter/2.png)

Please note that **this filter_setting.json file is for reference only. If it differs from the actual trading periods (due to adjustments by the exchange to contract trading periods), VeighNa official is not responsible**.

After the filter_setting.json file is generated, it will be placed in the .vntrader folder under the VeighNa Elite Trader running directory (usually the user directory).

### Manually Editing the File

If the number of traded varieties is small, you can manually create a filter_setting.json file and fill in the corresponding contract trading time configuration information (the K-line time periods allowed to be pushed into the strategy), as shown below:

```
{
    "IF": [
        ["9:30:00", "11:29:00"],
        ["13:00:00", "14:59:00"]
    ],
    "rb": [
        ["9:00:00", "10:14:00"],
        ["10:30:00", "11:29:00"],
        ["13:30:00", "14:59:00"],
        ["21:00:00", "22:59:00"]
    ],
    "au": [
        ["9:00:00", "10:14:00"],
        ["10:30:00", "11:29:00"],
        ["13:30:00", "14:59:00"],
        ["21:00:00", "23:59:59"],
        ["00:00:00", "2:29:00"]
    ]
}
```

Please note:
 - The datetime of K-lines in VeighNa is the start time of the K-line, not the end time;

 - If trading commodity futures, do not forget the break time from 10:15 to 10:30;

 - If the trading time of the contract involves overnight sessions, split the night session trading time into two segments: from the start time to 23:59:59 and from 00:00:00 to the end time.

After configuring the filter_setting.json file, place it in the .vntrader folder under the VeighNa Elite Trader running directory.

## Testing Filter Effect

If you want to test the effect of the data filtering function, you can add print statements in the strategy's on_history function to check if the strategy internally receives K-lines from non-trading periods, as shown below:

```python3
# Judge the live trading status, and only output after the strategy is started
if self.trading:
    self.write_log(f"{self.strategy_name}_{self.vt_symbol}：{hm.datetime[-1]}")
```
