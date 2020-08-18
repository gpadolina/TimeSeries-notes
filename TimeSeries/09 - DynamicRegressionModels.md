## Dynamic Regression Models
The time series models in the previous two chapters allow for the inclusion of information from past observations of a series, but not for the inclusion of other
that may also be relevant. For example, the effect of holidays, competitor activity, changes in the law, the wider economy, or other external variables, may explain
some of the historical variation and may lead to more accurate forecasts. In this chapter, we consider how to extend ARIMA models in order to allow other information
to be included in the models.

In this chapter, we will allow the error from a regression to contain autocorrelation. To emphasize this change in perspective, we will replace et with nt in the
equation. The error series nt is asummed to follow an ARIMA model. For example, if nt follow an ARIMA(1,1,1) model, we write,

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/ARIMA(1%2C1%2C1)%20model.png)

where et is a white noise series.

Notice that the model has two error terms here - the error from the regression model, which we denote by nt and the error from the ARIMA model, which we denote by et.
Only the ARIMA model errors are assummed to be white noise.
## 9.1 Estimation
When we estimate the parameters from the model, we need to minimize the sum of squared et values. IF we minimize the sum of squared nt values instead (which is what
would happen if we estimated the regression model ignoring the autocorrelations in the errors), then several problems arise.
1. The estimated coefficients b0,...,bk are no longer the best estimates, as some information has been ignored in the calculation.
2. Any statistical tests associated with the model (eg t-tests on the coefficients) will be incorrect.
3. The AICc values of the fitted models are no longer a good guide as to which is the best model for forecasting.
4. In most cases, the p-values associated with the coefficients will be too small and so some predictor variables will appear to be important when they are not.
This is known as "spurious regression."

Minimizing the sum of squared et values avoids these problems. Alternatively, maximum likelihood estimation can be used; this will give similar estimates of the
coefficients.

An important consideration when estimating a regression with ARMA erorrs is that all of the variables in the model must first be stationary. Thus, we first have
to check that yt and all of the predictors appear to be stationary. One exception to this is the case where non-stationary variables are co-integrated. If there
exists a linear combination of the non-stationary yt and the predictors that is stationary, then the estimated coefficients will be consistent.

We therefore first difference the non-stationary variables in the model. It is often desirable to maintain the form of the relationship between yt and the predictors
and consequently it is common to difference all of the variables if any of them need differencing. The resulting model is then called a "model in differences", as
distinct from a "model in levels", which is what is obtained when the original data are used without differencing.

If all of the variables in the model are stationary, then we only need to consider ARMA errors for the residuals. It is easy to see that a regression model with
ARIMA errors is equivalent to a regression model in differences with ARMA errors.
## Regression with ARIMA errors in R
The R function ```Arima( )``` will fit a regression model with ARIMA errors if the argument ```xreg``` is used. The order argument specifies the order of the ARIMA
error model.
```
fit <- Arima(y, xreg=x, order=c(1,1,0))
```
The ```auto.arima( )``` function will also handle regression terms via the ```xreg``` argument. The user must specify the predictor variables to include, but
```auto.arima( )``` will select the best ARIMA model for the errors. If differencing is required, then all variables are differenced during the estimation process,
although final model will be expressed in terms of the original variables.

The AICc is calculated for the final model and this value can be used to determine the best predictors.
## Forecasting
To forecast using a regression model with ARIMA errors, we need to forecast the regression part of the model and the ARIMA part of the model and combine the results.
As with ordinary regression models, in order to obtain forecasts we first need to forecast the predictors.
## Stochastic and deterministic trends
There are two different ways of modelling a linear trend. A *deterministic trend* is obtained using the regression model

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Deterministic%20trend.png)

where nt is an ARMA process. A *stochastic trend* is obtained using the model

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Deterministic%20trend.png)

where nt is an ARIMA process with d = a. In the latter case, we can difference both sides.
