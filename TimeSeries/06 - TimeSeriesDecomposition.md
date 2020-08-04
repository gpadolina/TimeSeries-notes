## Time Series Decomposition
Time series data can exhibit a variety of pattern, and it is helpful to split a time series in several components, each representing an underlying pattern.

When we combine a time series into components, we usually combine trend and cycle into a single trend-cycle compoenent sometimes called the trend for simplicity.
Thus, we think of a time series as comprising three components: a trend-cycle component, a seasonal component, and a remainder component containing anything else
in the time series.
## Time series components
If we assume an additive decomposition, then we can write ```y = S + T + R```, where y is the data, S is the seasonal component, T is the trend-cycle component,
and R is the remainder component, at all period t. Alternatively, a multiplicative decomposition would be written as ```y = S x T x R```.

The additive decomposition is the most appropriate if the magnitude of the seasonal fluctuations or the variation around the trend-cycle, does not vary with the
level of the time series. When the variation in the seasonal pattern, or the variation around the trend-cycle, appears to be proportional to the level of the time
series, then a multiplicative decomposition is more appropriate. Multiplicative decompositions are common with economic time series.

When a log transformation has been used, this is equivalenet to using a multiplicative decomposition because ```y = S x T x R``` is equivalent to 
```log y = log S + log T + log R```.
#### Seasonally adjusted data
If the seasonal component is removed from the original data, the resulting values are the "seasonally adjusted" data. For an additive decomposition, the seasonally
adjusted data are given by ```y - S```, and for multiplicative data, the seasonally adjusted values are obtained by using ```y/S```.
## Moving averages
The classical method of time series decomposition originated in the 1920s and was widely used until the 1950s. It still forms the basis of many time series
decomposition methods, so it is important to understand how it works. The first step in a classical decomposition is to use a moving average method to estimate the
trend-cycle, so we begin by discussing moving averages.
#### Moving average smoothing
A moving average of order m can be written as ``` \hat{T}_{t} = \frac{1}{m} \sum_{j=-k}^k y_{t+j}``` where m = 2k + 1. That is, the estimate of the trend-cycle at
time t is obtained by averaging values of the time series within k periods of t. The average eliminates some of the randomness in the data, leaving a smooth
trend-cycle component. We call this an m-*MA*, meaning a moving average of order m.

Moving averages can be computed using ```ma(timeseries, m)```

Example:
```
autoplot(elecsales, series="Data") + 
  autolayer(ma(elecsales,5), series="5-MA") + 
  xlab("Year") + ylab("GWh") +
  ggtitle("Annual electricity sales: South Australia") +
  scale_colour_manual(values=c("Data"="grey50","5-MA"="red"), 
                      breaks=c("Data","5-MA"))
```
The order of moving average determines the smoothness of the trend-cycle estimate. In general, a larger order means a smoother curve.
