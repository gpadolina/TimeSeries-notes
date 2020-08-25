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
