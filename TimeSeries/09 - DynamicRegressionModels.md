## Dynamic Regression Models
The time series models in the previous two chapters allow for the inclusion of information from past observations of a series, but not for the inclusion of other
that may also be relevant. For example, the effect of holidays, competitor activity, changes in the law, the wider economy, or other external variables, may explain
some of the historical variation and may lead to more accurate forecasts. In this chapter, we consider how to extend ARIMA models in order to allow other information
to be included in the models.

In this chapter, we will allow the error from a regression to contain autocorrelation. To emphasize this change in perspective, we will replace et with nt in the
equation. The error series nt is asummed to follow an ARIMA model. For example, if nt follow an ARIMA(1,1,1) model, we write,

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/ARIMA(1%2C1%2C1)%20model.png)
