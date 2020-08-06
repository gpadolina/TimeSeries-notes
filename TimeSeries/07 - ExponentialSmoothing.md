## Exponential Smoothing

Exponential smoothing was proposed in the late 1950s and has motivated some of the most successful forecasting methods. Forecasts produced using exponential smoothing
methods are weighted averages of past observations, with the weights decaying exponentially as the observations get older. In other words, the more recent the
observation the higher the associated weight.

In the first part, we present the mechanics of the most important exponential smoothing methods and their application in forecasting time series with various
characteristics. In the second part, we present the statistical models that underlie exponential smoothing methods.

## Simple exponential smoothing
The simplest of the exponentially smoothing methods is naturally called *simple exponential smoothing (SES)*. This method is suitable for forecasting data with no
clear trend or seasonal pattern.

Using the naïve method, all forecasts for the future are equal to the last observed value of the series. Hence, the naïve method assumes that the most recent observation
is the only important one and all previous observations provide no information for the future.

Using the average method, all future forecasts are equal to a simple average of the observed data. Hence, the average method assumes that all observations are of equal
importance and gives them equal weights when generating forecasts.

We often want something between these two extremes. For example, it may be sensible to attach larger weights to more recent observations than to observations from
the distant past. This is exacctly the concept behind simple exponential smoothing. Forecasts are calculated using weighted averages, where the weights decrease
exponentially as observations come from further in the past - the smallest weights are associated with the oldest observations.
#### Weighted average form
The forecast at time T + 1 is equal to a weighted average between the most recent observation yT and the previous forecast y(T|T-1).
#### Component form
An alternative representation is the component form. For simple exponential smoothing, the only component included is the level, lt. Other methods may also included a
trend bt and a seasonal component st. Component form representations of exponential smoothing methods comprise a forecast equation and a smoothing equation for each
of the components included in the method.

The component form of simple exponential smoothing is not particularly useful, but it will be the easiest form to use when we start adding other components.
#### Flat forecasts
Simple exponental smoothing has a "flat" forecast function. That is, all forecasts take the same value, equal to the last level component. Remember that these forecasts
will only be suitable if the time series has no trend or seasonal component.
#### Optimization
The application of every exponential smoothing method requires the smoothing parameters and the inital values to be chosen. In particular, for simple exponential
smoothing, we need to select the values of alpha and l0. All forecasts can be computed from the data once we know those values.

In some cases, the smoothing parameters may be chosen in a subjective manner. However, a more reliable and objective way to obtain values for the unknown parameters is
to estimate them from the observed data.

Regression model coefficients can be estimated by minimizing the sum of the squared residuals. Similarly, the unknown parameters and the initial values for any
exponential smoothing method can be estimated by minimizing the SSE.

Unlike regression where we have formulas which return the values of the regression coefficients that minimizes the SSE, this involves a non-linear minimization problem
and we need to use an optimization tool to solve it.
## Trend methods
#### Holt's linear trend method
Holt extended simple exponential smoothing to allow the forecasting of data with a trend. This method involves a forecast equation and two smoothing equations, one for
the level and one for the trend: forecast equation, level equation, and trend equation.
```
holt(timeseries, h=)
```
The forecast function is no longer flat but trending. The h-step-ahead forecast is equal to the last estimated level plus h times the last estimated trend value. Hence,
the forecasts are a linear function of h.
