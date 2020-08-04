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

Moving averages can be computed using ```ma(timeseries, order=m)```

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
#### Moving averages of moving averages
It is possible to apply a moving average to a moving average. One reason for doing this is to make an even-order moving average symmetric. When a 2-MA follows a
moving average of an even order (such as 4), it is called a "centred moving average of order 4). This is because the results are now symmetric.

By default, the ```ma( )``` function in R will return a centered moving average for even orders unless ```center=FALSE``` is specified.

In general, an even order MA should be followed by an even order MA to make it symmetric. Similarly, an odd order MA should be followed by an odd order MA.
#### Estimating the trend-cycle with seasonal data
The most common use of centered moving averages is for estimating the trend-cycle from seasonal data.

When applied to quarterly data, each quarter of the year is given equal weight as the first and last terms apply to the same quarter in consecutive years.
Consequently, the seasonal variation will be averaged out and the resulting values of T will have little or no seasonal variation remaining.

In general, a 2 x m-MA is equivalent to a weighted moving average of order m + 1 where all observations to take the weight 1/m, except for the first and last terms
which take weights 1/(2m). So, if the seasonal period is even and of order m, we use a 2 x m-MA to estimate the trend-cycle. If the seasonal period is odd and of
order m, we use a m-MA to estimate the trend-cycle.
#### Weighted moving averages
Combinations of moving averages result in weighted moving averages. In general, a weighted m-MA can be written as
```
\hat{T}_t = \sum_{j=-k}^k a_j y_{t+j},
```
where k = (m - 1) / 2, and the weights are given by(a-k,...ak). It is important that the weights all sum to one and that they are symmetric so that a sub(j)=a sub(-j).

A major advantage of weighted moving averages is that they yield a smoother estimate of the trend-cycle. Instead of observations entering and leaving the calculation
at full weight, their weight slowly increase and then slowly decrease, resulting in a smoother curve.
## Classical decomposition
The classical decomposition method originated in the 1920s. It is a relatively simple procedure, and forms the starting point for most other methods of time series
decomposition. There are two forms of classical decomposition: an additive and a multiplicative decomposition.

In classical decomposition, we assume that the seasonal component is constant from year to year. For multiplicative seasonality, the m values that form the seasonal
component are sometimes called the *seasonal indices*.
#### Additive decomposition
* Step 1 - If m is an even order, compute the trend-cycle component T using a 2 x m-MA. IF m is an odd order, compute the trend-cycle component T using m-MA.
* Step 2 - Calculate the detrended series: y - T.
* Step 3 - To estimate the seasonal component for each season, simply average the detrended values for that season. The seasonal component is obtained by stringing
together these monthly values, and then replicating the sequence for each year of data. This gives S.
* Steo 4 - The remainder component is calculated by subtracting the estimated seasonal and trend-cycle components: R = y - T - S.
#### Multiplicative decomposition
A classical multiplicative decomposition is similar, except that the subtractions are replaced by divisions.
* Step 1 - If m is an even number, compute the trend-cycle component T using a 2 x m-MA. If m is an odd number, compute the trend-cycle component T using m-MA.
* Step 2 - Calculcate the detrended series: y / T.
* Step 3 - To estimate the seasonal component for each season, simply average the detrended values for that season. The seasonal component is obtained by stringing
together these monthly indexes, and then replicating the sequence for each year of data. This gives S.
* Step 4 - The remainder component is calculated by dividing out the estimated seasonal and trend-cycle components: R = y / (TS)
#### Components on classical decomposition
While classical decomposition is still widely used, it is not recommended, as there are now several much better methods. Some of the problems with classical
decomposition are summarised as follows:
