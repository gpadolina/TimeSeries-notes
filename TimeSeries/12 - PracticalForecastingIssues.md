## Practical forecasting issues

## Weekly, daily, and sub-daily data
Weekly, daily, and sub-daily data can be challenging for forecasting, although for different reasons.
#### Weekly data
Weekly data is difficult to work with because the seasonal time period (the number of weeks in a year) is both large and non-integer. The average number of weeks in
a year is 52.18. Most of the methods we have considered require the seasonal period to be an integer. Even if we approximate it by 52, most of the methods will not
handle such a large seasonal period efficiently.

The simplest approach is to use an STL decomposition along with a non-seasonal method applied to the seasonally adjusted data. For example, 
```
gasoline %>% stlf( ) %>% autoplot( )
```
An alternative approach is to use a dynamic harmonic regression model. In the following example, the number of Fourier terms was selected by minimizing the AICc. The
order of the ARIMA model is also selected by minimizing the AICc, although that is done within the ```auto.arima( )``` function.
```
bestfit <- list(aicc=Inf) 
for(K in seq(25)) {
  fit <- auto.arima(gasoline, xreg=fourier(gasoline, K=K), 
    seasonal=FALSE)
  if(fit[["aicc"]] < bestfit[["aicc"]]) { 
    bestfit <- fit
    bestK <- K
  } 
}
fc <- forecast(bestfit, 
  xreg=fourier(gasoline, K=bestK, h=104))
autoplot(fc)
```
The STL approach or TBATS model is preferable when the seasonality changes over time. The dynamic harmonic regression approach is preferable if there are covariates
that are useful predictors as these can be added as additional regressors.
#### Daily and sub-daily data
Daily and sub-daily data are challenging for a different reason - they often involve multiple seasonal patterns and so we need to use a method that handles such
complex seasonality.

When the time series is long enough so that some of the longer seasonal periods become apparent, it will be necessary to use STL, dynamic harmonic regression or TBATS.

However, note that even these models only allow for regular seasonality. Capturing seasonality associated with moving events such as Easter, Id, or the Chinese New
Year is more difficult.

The best way to deal with moving holiday effect is to use dummy variables. However, neither STL, ETS nor TBATS models allow for covariates. Amongst the models discussed,
the only choise is a dynamic regression model, where the predictors include any dummy holiday effects and possibly also the seasonality using Fourier terms.

## Time series of counts
All of the methods discussed assume that the data have a continuous sample space. But often, data comes in the form of counts.

In practice, this rarely matters provided our counts are sufficiently large. If the minimum number of customers is at least 100, then the different between a continuous
sample space [100, inf] and the discrete sample space {100, 101, 102,...} has no perceivable effect on our forecasts. However, if our data contains small counts
(0, 1, 2,...), then wee need to use forecasting method that are more appropriate for a sample space of non-negative integers.

There is one simple method which gets used in this context. It is "Croston's method," named afters its British inventor, John Croston. This method does not properly
deal with the count nature of the data either, but it is used so often that it is worth knowing about it.

With Croston's method, we construct two new series from our original time series by noting which time periods contain zero values and which periods contain non-zero
values. Croston's method involves separate simple exponential smoothing forecasts on the two new series a and q. Because the method is usually applied to time series
of demand for items, q is often called the "demand" and a the "inter-arrival time."

The ```croston( )``` function produces forecasts using Croston's method. It simply uses alpha = 0.1 by default and l0 is set to be equal to the first observation
in each of the method series.

An implementation of Croston's method with more facilities including parameter estimation is available in the *tsintermittent package* for R.

## Ensuring forecasts stay within limits
It is common to want forecasts to be positivve or to require them to be within some specified range [a, b]. Both of these situations are relatively easy to handle
using transformations.
#### Positive forecasts
To impose a positive constraint, simple work on the log scale, by specifying the Box-Cox parameter lamdda=0.
```
eggs %>%
  ets(model="AAN", damped=FALSE, lambda=0) %>% 
  forecast(h=50, biasadj=TRUE) %>%
  autoplot()
```
Because we set ```biasadj=TRUE```, the forecasts are the means of the forecast distributions.
#### Forecasts constrained to an interval
TO see how to handle data constrained to an interval, imagine that the prices were constrained to lie within a = 50 and b = 400. Then we can transform the data using
a scaled logit transform which maps(a, b) to the whole real line.

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Logit%20transform.png)

where x is on the original scale and y is the transformed data. To reverse the transformation, we will use

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Reverse%20logit%20transform.png)

## Forecast combinations
An easy way to improve forecast accuracy is to use several different methods on the same time series and to average the resulting forecasts. Bates and Granger wrote
a famous paper showing that combining forecasts often leads to better forecast accuracy.

While there has been considerable research on using weighted averages or some other more complicated combination approach, using a simple average has proven hard to
beat.

Here is an example using ETS, ARIMA, STL-ETS, NNAR, and TBATS.
```
train <- window(auscafe, end=c(2012,9))
h <- length(auscafe) - length(train)
ETS <- forecast(ets(train), h=h)
ARIMA <- forecast(auto.arima(train, lambda=0, biasadj=TRUE),
  h=h)
STL <- stlf(train, lambda=0, h=h, biasadj=TRUE) 
NNAR <- forecast(nnetar(train), h=h)
TBATS <- forecast(tbats(train, biasadj=TRUE), h=h) 
Combination <- (ETS[["mean"]] + ARIMA[["mean"]] +
  STL[["mean"]] + NNAR[["mean"]] + TBATS[["mean"]])/5

autoplot(auscafe) +
  autolayer(ETS, series="ETS", PI=FALSE) + 
  autolayer(ARIMA, series="ARIMA", PI=FALSE) + 
  autolayer(STL, series="STL", PI=FALSE) + 
  autolayer(NNAR, series="NNAR", PI=FALSE) + 
  autolayer(TBATS, series="TBATS", PI=FALSE) + 
  autolayer(Combination, series="Combination") + 
  xlab("Year") + ylab("$ billion") +
  ggtitle("Australian monthly expenditure on eating out")
```
TBATS does particularly well with this series, but the combination approach is even better. For other data, TBATS may be quite poor, while the combination approach is
almost always close to, or better than, the best component method.
## Prediction intervals for aggregates
A common problem is to forecast the aggregate of several time periods of data, using a model fitted to the disaggregated data. For example, we may have monthly data
but wish to forecast the total for the next year.

If the point forecasts are means, then adding them up will give a good estimate of the total. But prediction intervals are more trickly due to the correlations
between forecast errors.

A general solution is to use simulations.
## Backcasting
Sometimes it is useful to "backcast" a time series - that is, forecast in reverse time. Although there no built-in R functions to do this, it is easy to implement.
THe following functions reverse a ```ts``` object and a ```forecast``` object.
```
# Function to reverse time
reverse_ts <- function(y) 
{
ts(rev(y), start=tsp(y)[1L], frequency=frequency(y)) 
}
# Function to reverse a forecast
reverse_forecast <- function(object) 
{
  h <- length(object[["mean"]])
  f <- frequency(object[["mean"]]) 
  object[["x"]] <- reverse_ts(object[["x"]]) 
  object[["mean"]] <- ts(rev(object[["mean"]]),
    end=tsp(object[["x"]])[1L]-1/f, frequency=f) 
  object[["lower"]] <- object[["lower"]][h:1L,] 
  object[["upper"]] <- object[["upper"]][h:1L,] 
  return(object)
}
```
