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

The same stationarity and invertibility conditions that are used for autoregressive and moving average models also apply to an ARIMA model.

We have some special cases of the ARIMA models:

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Special%20cases%20of%20ARIMA.png)

Selecting appropriate values for p, d, and q can be difficult. However, the ```auto.arima( )``` or ```ARIMA( )``` function in R will do it for you automatically.

#### Understanding ARIMA models
The ```auto.arima( )``` function is useful, but anything automated can be a little dangerous and it is worth understanding something of the behavior of the models
even when you rely on an automatic procedure to choose the model for you.

The constant c ahs an important effect on the long-term forecasts obtained from these models.

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Constant%20C%20effect%20ARIMA.png)

The value of d also has an effect on the prediction intervals - the higher the value of d, the more rapidly the prediction intervals increase in size. For d = 0,
the long-term forecast standard deviation will go to the standard deviation of the historical data, so the prediction intervals will all be essentially the same.

The value of p is important if the data show cycles. To obtain cyclic forecasts, it is necessary to have p >= 2, along with some additional conditions on the
parameters.

#### ACF and PACF plots
It is usually not possible to tell, simply from a time plot, what values of p and q are appropriate for the data. However, it is sometimes possible to use the
ACF plot and the closely related PACF plot, to determine appropriate values for p and q.

To overcome this problem, we can use *partial autocorrelations*. These measure the relationship between yt and yt-k after removing the effects of lags. So the
first partial autocorrelation is identical to the first autocorrelation because there is nothing between them to remove. Each partial autocorrelation can be
estimated as the last coefficient in an autoregressive model.

If the data are from an ARIMA(p,d,0) or ARIMA(0,d,q) model, then the ACF and PACF plots can be helpful in determining the value of p or q. If p and q are both
positive, then the plots do not help in finding suitable values of p and q.

The data may follow an ARIMA(p,d,0) model if the ACF and PACF plots of the differenced data show the following patterns:
* the ACF is exponentially decaying or sinusoidal
* there is a significant spike at lag p in the PACF, but none beyond lag p.
The data may follow an ARIMA(0,d,q) model if the ACF and PACF plots of the differenced data show the following patterns:
* the PACF is exponentially decaying or sinusoidal
* there is a significant spike at lag q in the ACF, but none beyond lag q.

## Estimation and order selection
#### Maximum Likelihood estimation
Once the model order has been identified(ie the values of p,d,and q), we need to estimate the parameters c. When R estimates the ARIMA model, it uses maximum
likelihood estimation (MLE). This technique finds the values of the parameters which maximise the probability of obtaining the data that we have observed. For
ARIMA models, MLE is similar to the least squares estiamtes that would be obtained by minimizing

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/MLE.png)

Note that ARIMA models are much complicated to estimate than regression models and different software will give slightly different answers as they use different
methods and different optimization algorithms.

In practice, R will report the value of the log likelihood of the data; that is, the logarithm of the probability of the observed data coming from the estimated
model. For given values of p, d, and q, R will try to maximize the log likelihood when finding parameter estimates.

#### Information Criteria
Akaike's Information Criterion (AIC), which was useful in selecting predictors for regression, is also useful for determining the order of an ARIMA model. It can
be written as

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/AIC%20ARIMA.png)

For ARIMA models, the corrected AIC can be written as

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/AICc%20ARIMA.png)

and the Bayesian Information Criterion can be written as

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/BIC%20ARIMA.png)

It is important to note that these information criteria tend to not be good guides to selecting the appropriate order of differencing (d) of a model, but only for
selecting the values of p and q. This is because the differencing changes the data on which the likelihood is computed, making the AIC values between models with
different orders of differencing not comparable. So we need to use some other approach to choose d and then we can use the AICc to select p and q.

## ARIMA modelling in R
#### How does ```auto.arima( )``` work?
The ```auto.arima( )``` function in R uses a variation of the Hyndman-Khandakar algorithm, which combines unit root tests, minimization of the AICc and MLE to
obtain an ARIMA model. The arguments to ```auto.arima( )``` provide many variations on the algorithm.

These approximations can be avoided with the argument ```approximation=FALSE```. It is possible that the minimum AICc model will not be found due to these
approximations or because the use of a stepwise procedure. A much larger set of models will be search if the argument ```stepwise=FALSE``` is used.
#### Choosing your own model
If you want to choose the model yourself, use the ```auto.arima( )``` function wiht a single value for input for ```pdq( )``` and ```PDQ( )```. There is another
function ```arima( )``` in R which also fits an ARIMA model. However, it does not allow for the constant c unless d=0 and it does not return everything required
for other functions in the *forecast* package to work. Finally, it does not allow the estimated model to be applied to new data (which is useful for checking
forecast accuracy). Consequently, it is recommended that ```ARIMA( )``` be used instead.
#### Modelling procedure
When fitting an ARIMA model to a set of non-seasonal time series data, the following procedure provides a useful general approach.
1. Plot the data and identify any unusual observations.
2. If necessary, transform the data using a Box-Cox transformation to stablize the variance.
3. IF the data are non-stationary, take first differences of the data until the data are stationary.
4. Examine the ACF/PACF: Is an ARIMA(p,d,0) or ARIMA(0,d,q) model appropriate?
5. Try your chosen model(s) and use the AICc to search for a better model.
6. Check the residuals from your chosen model by plotting the ACF of the residuals and doing a portmanteau teast of the residuals. If they do not look like white
noise, try a modified model.
7. Once the residuals look like white noise, calculate forecasts.

## Forecasting
#### Point forecasts
Although we have calculated forecasts from the ARIMA models in our examples, we have not yet explained how they are obtained. Point forecasts can be calculated using
the following three steps.
1. Expand the ARIMA equation so that yt is on the left hand side and all other terms are on the right.
2. Rewrite the equation by replacing t with T + h.
3. On the right hand side of the equation, replace future observations with their forecasts, future errors with zero, and past errors with the corresponding residuals.

#### Prediction intervals
The calculation of ARIMA prediction intervals is more difficult.

The first prediction interval is easy to calculate. If small sigma is the standard deviation of the residuals, then a 95% prediction interval is given by

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/95%25%20Prediction%20Interval.png)

This result is true for all ARIMA models regardless of their parameters and orders.

Multi-step prediction intervals for ARIMA(0,0,q) models are relatively easy to calculate. We can write the model as

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Multi-step%20Prediction%20Interval.png)

Then, the estimated forecast variance can be written as

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Estimated%20Forecast%20Variance.png)

More general results and other special cases of multi-step prediction intervals for an ARIMA(p,d,q) model are given in more advanced textbooks.

The prediction intervals for ARIMA models are based on assumptions that the residuals are uncorrelated and normally distributed. If either of these assumptions
does not hold, then the prediction intervals may be incorrect. For this reason, always plot the ACF and histogram of the residuals to check the assumptions
before producing prediction intervals.

As with most prediction interval calculations, ARIMA-based intervals tend to be too narrow. This occurs because only the variation in the rrors has been accounted
for.

## Seasonal ARIMA models
So far, we have restricted our attention to non-seasonal data and non-seasonal ARIMA models. However, ARIMA models are also capable of modelling a wide range of
seasonal data.

A seasonal ARIMA model is formed by including additional seasonal terms in the ARIMA models we have seen so far written as follows:

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Seasonal%20ARIMA%20model.png)

where m = number of observations per year. We use the uppercase notation for the seasonal parts of the model and lowercase notation for the non-seasonal parts of the
model.
#### ACF/PACF
The seasonal part of an AR or MA model will be seen in the seasonal lags of the PACF and ACF. For example, an ARIMA(0,0,0)(0,0,1)12 model will show:
* a spike at lag 12 in the ACF but no other significant spikes
* exponential decay in the seasonal lags of the PACF(ie lags at 12, 24, 36...)
Similarly, an ARIMA(0,0,0)(1,0,0)12 model will show:
* exponential decay in the seasonal lags of the ACF
* a single significant spike at 12 in the PACF
## ARIMA vs ETS
It is commonly held myth that ARIMA models are more general than exponential smoothing. While linear exponential smoothing models are all special cases of ARIMA
model, the non-linear exponential smoothing models have no equivalent ARIMA counterparts. On the other hand, there are also many ARIMA models that have no exponential
smoothing counterparts. In particular, all ETS model are non-stationary, while some ARIMA models are stationary.

The ETS models with seasonality or non-damped trend or both have two unit roots (they need two levels of differencing to make them stationary). All other ETS models
have one unit root (they need one level of differencing to make them stationary).

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Equivalence%20relationships%20between%20ETS%20and%20ARIMA%20models.png)
