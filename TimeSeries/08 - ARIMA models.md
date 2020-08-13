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
#### Unit root tests
One way to determine more objectively whether differencing is required is to use a unit root test. These are statistical hypothesis tests of stationarity that are
designed for determining whether differencing is required.

A number of unit root tests are available, which are based on different assumptions and may lead to conflicting answers. In our analysis, we use the *Kwiatkowski-
Phillips-Schmidt-Shin (KPSS)* test. In this test, the null hypothesis is that the data are stationary and we look for evidence that the null hypothesis is false.
Consequently, small p-values less than 0.05 suggest that differencing is required. The test can be computed using the ```ur.kpss( )``` function from the urca package.
```
library(urca)
ur.kpss( ) %>% summary( )
```
## Backshift notation
The backward shift operator B is a useful notational device when working with time series lags. In other words, B, operating on yt, has the effect of shifting the data
back one period. Two applications of B to yt shifts the data back two periods.

The backward shift operator is convenient for describing the process of differencing. A first difference can be written as

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/First%20difference%20backward%20shift.png)

Note that a first difference is represented by (1-B). Similarly, if second-order differences have to be computed, then:

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Second-order%20differences.png)

## Autoregressive models
In a multiple regression model, we forecast the variable of interest using a linear combination of predictors. In an autoregression model, we forecast the variable of
interest using a linear combination of past values of the variable. The term autoregression indicates that it is a regression of the variable against itself.

Thus, an autoregressive model of order p can be written as

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Autoregressive%20model.png)

where et is white noise. This is like a multiple regression but with *lagged values* of yt as predictors. We refer to this as an *AR(p) model*, an autoregressive
model of order p.

For an AR(1) model:

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/AR(1)%20model.png)

We normally restrict autoregressive models to stationary data, in which case some constraints on the values of the parameters are required.

![parameters](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/AR%20parameters.png)

When p>=3, the restrictions are much more complicated.

## Moving average models
Rather than using past values of the forecast variable in regression, a moving average model uses past forecast errors in a regression like model.

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Moving%20average%20model.png)

We refer to this as an *MA(q) model*, a moving average model of order q. Of course, we do not observe the values of et, so it is not really a regression in
the usual sense.

Notice that each value of yt can be thought of as a weighted moving average of the past few forecast errors. However, moving average models should not be
confused with the moving average *smoothing*. A moving average mdel is used for forecasting future values, while moving average smoothing is used for
estimating the trend-cycle of past values.

It is possible to write any stationary AR(p) model as an MA(infinity) model. For example, using repeated substitution, we can demonstrate this for an AR(1) model:

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/AR(1)%20model%20repeated%20substitution.png)

The reverse result holds if we impose some constraints on the MA parameters. Then the MA model is called *invertible*. That is, we can write any invertible MA(q)
process as an AR(infinity) process. Invertible models are not simply introduced to enable us to convert from MA models to AR models. They also have some desirable
mathematical properties.

## Non-seasonal ARIMA models
If we combine differencing with autoregression and a moving average model, we obtain a non-seasonal ARIMA model. ARIMA is an acronym for AutoRegressive Integrated
Moving Average. In this context, integration is the reverse of differencing. The full model can be written as

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Non-seasonal%20ARIMA%20model.png)

The predictors on the right hand side include both lagged values of yt and lagged errors. We call this an *ARIMA(p,d,q) model* where
* p = order of the autoregressive part
* d = degree of first differencing involved
* q = order of the moving average part
