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
