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
```log y = log S + log T + log R```