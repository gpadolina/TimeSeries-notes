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
