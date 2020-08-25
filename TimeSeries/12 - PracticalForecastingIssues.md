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
sample space ```[100, inf]``` and the discrete sample space {100, 101, 102,...} has no perceivable effect on our forecasts. However, if our data contains small counts
(0, 1, 2,...), then wee need to use forecasting method that are more appropriate for a sample space of non-negative integers.

There is one simple method which gets used in this context. It is "Croston's method," named afters its British inventor, John Croston. This method does not properly
deal with the count nature of the data either, but it is used so often that it is worth knowing about it.

With Croston's method, we construct two new series from our original time series by noting which time periods contain zero values and which periods contain non-zero
values. Croston's method involves separate simple exponential smoothing forecasts on the two new series a and q. Because the method is usually applied to time series
of demand for items, q is often called the "demand" and a the "inter-arrival time."

The ```croston( )``` function produces forecasts using Croston's method. It simply uses alpha = 0.1 by default and l0 is set to be equal to the first observation
in each of the method series.

An implementation of Croston's method with more facilities including parameter estimation is available in the *tsintermittent package* for R.
