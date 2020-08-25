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
