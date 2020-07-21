## Simple forecasting methods

#### Average method
The forecasts of all future values are equal to the average (or "mean") of the historical data.
```
meanf(y, h)
# y contains the time series
# h is the forecast horizon
```
#### Naïve method
For naïve method, simply set all forecasts to be the value of the last observation. This method works remarkably well for many
economic and financial time series. Because a naïve forecast is optimal when data follow a random walk, these are also called
*random walk forecasts.*
```
naive(y, h)
rwf(y, h) # Equivalent alternative
```
#### Seasonal naïve method
A similar method is useful for highly seasonal data. In this case, set each forecast to be equal to the last observed value
from the same season of the year (eg., the same month of the previous year).
```
snaive(y, h)
```
#### Drift method
A variation on the naïve method is to allow the forecasts to increase or decrease over time, where the amount of change over
time (called the drift) is set to be the average change seen in the historical data.
```
rwf(y, h, drift=TRUE)
```
#### Examples
```
autoplot( ) +
  autolayer(meanf( ), series="Mean") +
  autolayer(naive( ), series="Naive") +
  autolayer(snaive( ), series="Seasonal naive") +
  ggtitle( ) +
  xlab( ) +
  ylab( ) +
  guides( )
```
Sometimes one of these simple methods will be the best forecasting method available; but in many cases, these methods
will serve as benchmarks rather than the method of choice.

## Transformations and adjustments
Adjusting the historical data can often lead to a simpler forecasting task. There are four kinds of adjustments:
calendar adjustments, population adjustments, inflation adjustments, and mathematical transformations. The purpose
of these adjustments and transformations is to simplify the patterns in the historical data by removing known
sources of variation or by making the pattern more consistent across the whole data set. Simpler patterns usually
lead to more accurate forecasts.
