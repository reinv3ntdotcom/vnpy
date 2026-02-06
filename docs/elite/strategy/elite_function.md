# Indicator Calculation Functions

VeighNa Elite Trader's CTA strategy module has the following built-in calculation functions for strategy calls:

**sma**: Simple Moving Average

* Inputs
  * close: np.ndarray
  * time_period: int=30

* Outputs
  * sma_array: np.ndarray

**ema**: Exponential Moving Average

* Inputs
  * close: np.ndarray
  * time_period: int=30

* Outputs
  * ema_array: np.ndarray

**kama**: Adaptive Moving Average

* Inputs
  * close: np.ndarray
  * time_period: int=30

* Outputs
  * atr_array: np.ndarray

**wma**: Weighted Moving Average

* Inputs
  * close: np.ndarray
  * time_period: int=30

* Outputs
  * wma_array: np.ndarray

**apo**: Absolute Price Oscillator

* Inputs
  * close: np.ndarray
  * fast_period: int=12
  * slow_period: int=26
  * matype: int=0

* Outputs
  * apo_array: np.ndarray

Please note that matype corresponds to: 0=SMA, 1=EMA, 2=WMA, 3=DEMA, 4=TEMA, 5=TRIMA, 6=KAMA, 7=MAMA, 8=T3

**cmo**: Chande Momentum Oscillator

* Inputs
  * close: np.ndarray
  * time_period: int=14

* Outputs
  * cmo_array: np.ndarray

**mom**: Momentum

* Inputs
  * close: np.ndarray
  * time_period: int=10

* Outputs
  * mom_array: np.ndarray

**ppo**: Percentage Price Oscillator

* Inputs
  * close: np.ndarray
  * fast_period: int=12
  * slow_period: int=26
  * matype: int=0

* Outputs
  * ppo_array: np.ndarray

**roc**: Rate of Change

* Inputs
  * close: np.ndarray
  * time_period: int=10

* Outputs
  * roc_array: np.ndarray

**rocr**: Rate of Change Ratio

* Inputs
  * close: np.ndarray
  * time_period: int=10

* Outputs
  * rocr_array: np.ndarray

**rocp**: Rate of Change Percentage

* Inputs
  * close: np.ndarray
  * time_period: int=10

* Outputs
  * rocp_array: np.ndarray

**trix**: Triple Exponential Moving Average One-Day Rate of Change

* Inputs
  * close: np.ndarray
  * time_period: int=30

* Outputs
  * trix_array: np.ndarray

**stddev**: Standard Deviation

* Inputs
  * close: np.ndarray
  * time_period: int=5
  * nbdev: float=1

* Outputs
  * stddev_array: np.ndarray

**std**: Standard Deviation

* Inputs
  * close: np.ndarray
  * time_period: int=5
  * nbdev: float=1

* Outputs
  * std_array: np.ndarray

**obv**: On-Balance Volume

* Inputs
  * close: np.ndarray
  * volume: np.ndarray

* Outputs
  * obv_array: np.ndarray

**cci**: Commodity Channel Index

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * time_period: int=14

* Outputs
  * cci_array: np.ndarray

**atr**: Average True Range

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * time_period: int=14

* Outputs
  * atr_array: np.ndarray

**natr**: Normalized Average True Range

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * time_period: int=14

* Outputs
  * natr_array: np.ndarray

**rsi**: Relative Strength Index

* Inputs
  * close: np.ndarray
  * time_period: int=14

* Outputs
  * rsi_array: np.ndarray

**macd**: Moving Average Convergence Divergence

* Inputs
  * close: np.ndarray
  * fast_period: int=12
  * slow_period: int=26
  * signal_period: int=9

* Outputs
  * macd_array: np.ndarray
  * macdsignal_array: np.ndarray
  * macdhist_array: np.ndarray

**adx**: Average Directional Index

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * time_period: int=14

* Outputs
  * adx_array: np.ndarray

**adxr**: Average Directional Movement Index Rating

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * time_period: int=14

* Outputs
  * adxr_array: np.ndarray

**minus_di**: Minus Directional Indicator

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * time_period: int=14

* Outputs
  * minusdi_array: np.ndarray

**plus_di**: Plus Directional Indicator

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * time_period: int=14

* Outputs
  * plusdi_array: np.ndarray

**willr**: Williams' %R

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * time_period: int=14

* Outputs
  * willr_array: np.ndarray

**ultosc**: Ultimate Oscillator

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * time_period: int=7
  * time_period2: int=14
  * time_period3: int=28

* Outputs
  * ultosc_array: np.ndarray

**trange**: True Range

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray

* Outputs
  * trange_array: np.ndarray

**aroon**: Aroon Indicator

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * time_period: int=14

* Outputs
  * aroonup_array: np.ndarray
  * aroondown_array: np.ndarray

**aroonosc**: Aroon Oscillator

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * time_period: int=14

* Outputs
  * aroonosc_array: np.ndarray

**minus_dm**: Minus Directional Movement

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * time_period: int=14

* Outputs
  * minusdm_array: np.ndarray

**plus_dm**: Plus Directional Movement

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * time_period: int=14

* Outputs
  * plusdm_array: np.ndarray

**mfi**: Money Flow Index

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * volume: np.ndarray
  * time_period: int=14

* Outputs
  * mfi_array: np.ndarray

**ad**: Accumulation/Distribution Line

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * volume: np.ndarray

* Outputs
  * ad_array: np.ndarray

**adosc**: Chaikin Oscillator

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * volume: np.ndarray
  * fast_period: int=3
  * slow_period: int=10

* Outputs
  * adosc_array: np.ndarray

**bop**: Balance of Power

* Inputs
  * open: np.ndarray
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray

* Outputs
  * bop_array: np.ndarray

**stoch**: Stochastic Oscillator

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * fastk_period: int=5
  * slowk_period: int=3
  * slowk_matype: int=0
  * slowd_period: int=3
  * slowd_matype: int=0

* Outputs
  * slowk_array: np.ndarray
  * slowd_array: np.ndarray

**boll**: Bollinger Bands

* Inputs
  * data: np.ndarray
  * window: int
  * dev: float

* Outputs
  * bollup_array: np.ndarray
  * bolldown_array: np.ndarray

**keltner**: Keltner Channel

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * close: np.ndarray
  * window: int
  * dev: float

* Outputs
  * kkup_array: np.ndarray
  * kkdown_array: np.ndarray

**donchian**: Donchian Channel

* Inputs
  * high: np.ndarray
  * low: np.ndarray
  * window: int

* Outputs
  * donchianup_array: np.ndarray
  * donchiandown_array: np.ndarray

**cross_over**: Cross Above

* Inputs
  * data: np.ndarray
  * level: float

If the previous value of data is less than or equal to level and the latest value of data is greater than level, then return True.

* Outputs
  * cross_over: bool

**cross_below**: Cross Below

* Inputs
  * data: np.ndarray
  * level: float

* Outputs
  * cross_below: bool

If the previous value of data is greater than or equal to level and the latest value of data is less than level, then return True.

**check_increasing**: Check if Sequence is Monotonically Increasing

* Inputs
  * data: np.ndarray

* Outputs
  * increasing: bool

**check_decreasing**: Check if Sequence is Monotonically Decreasing

* Inputs
  * data: np.ndarray

* Outputs
  * decreasing: bool

**resample_data**: Resample Candlestick Data

* Inputs
  * df: pd.DataFrame
  * rule: str

* Outputs
  * resampled_df: pd.DataFrame

* Example

If you want to test the effect of the resample_data function, you can first get the candlestick DataFrame when the strategy's on_history function receives the hm push, then call the resample_data function to resample the candlestick data, as shown below:

```python3
# Determine the live trading status; only output after the strategy starts
df: pd.DataFrame = hm.to_dataframe()
resampled_df: pd.DataFrame = resample_data(df, "5min")
```
