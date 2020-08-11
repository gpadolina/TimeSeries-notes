## ARIMA models
ARIMA models provide another approach to time series forecasting. Exponential smoothing and ARIMA models are the two most widely used approaches to time series
forecasting and provide complementary approaches to the problem. While exponential smoothing models are based on a description of the trend and seasonality
in the data, ARIMA models aims to describe the autocorrelations in the data.
## Stationarity and differencing
A stationary time series is one whose properties do not depend on the time at which the series is observed. Thus, time series with trends or with seasonality are not
stationary - the trend and seasonality will affect the value of the time series at different times. On the other hand, a white noise series is stationary - it does
not matter when you observe it, it should look much the same at any point in time.

Some cases can be confusing - a time series with cyclic behavior but with no trend or seasonality is stationary. This is because the cycles are not of a fixed
length, so before we observe the series we cannot be sure where the peaks and troughs of the cycles will be.

In general, a stationary time series will have no predictable patterns in the long-term. Time plots will show the series to be roughly horizontal although some
cyclic behavior is possible, with constant variance.
#### Differencing
To make a non-stationary time series stationary - compute the differences between consecutive observations. This is known as *differencing*.

Transformations such as logarithms can help to stablize the variance of a time series. Differencing can help stablize the mean of a time series by removing changes
in the level of a time series and therefore, eliminating or reducing trend and seasonality.

The ACF plot is useful for identifying non-stationary time series. For a stationary time series, the ACF will drop to zero relatively quickly, while the ACF of
non-stationary data decreases slowly. Also, for non-stationary data, the value of r1 is often large and positive.
#### Random walk model
The differenced series is the change between the consecutive observations in the original series and can be written as

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Difference%20series.png)

When the differenced series is white noise, the model for the original series can be written as

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/White%20noise%20difference%20series.png)

where et denotes white noise. Rearranging leads to the "random walk" model

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Random%20walk%20model.png)

Random walk models are widely used for non-stationary data, particularly financial and economic data. Random walks typically have:
* long periods of apparent trends up or down
* sudden and unpredictable changes in direction
The forecasts from a random walk model are equal to the last observation, as future movements are unpredictable and are equally likely to be up or down. Thus, the
random walk model underpins naïve forecasts.
#### Second-order differencing
Occasionally the differenced data will not appear to be stationary and it may be necessary to difference the data a second time to obtain a stationary series:

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Second-order%20differencing.png)

In practice, it is almost never necessary to go beyong second-order differences.
#### Seasonal differencing
A seasonal difference is the difference between an observation and the previous observation from the same season.

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Seasonal%20differencing.png)

Sometimes it is necessary to take both a seasonal difference and a first difference to obtain stationary data.

When both seasonal and first differences are applied, it makes no difference which is done first - the result will be the same. However, if the data have a strong
seasonal pattern, we recommend that seasonal differencing be done first because the resulting series will sometimes be stationary and there will be no need for a
further first difference.

It is important that if differencing is used, the differences are interpretable. First differences are the change between one observation and the next. Seasonal
differences are the change between one year to the next.
