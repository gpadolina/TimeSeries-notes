## Advanced forecasting methods

## Complex seasonality
So far, we have considered relatively simple seasonal pattern such as quarterly and monthly data. However, higher frequency time series often exhibit more
complicated seasonal patterns. For example, daily data may have a weekly pattern as well as an annual pattern. Hourly data usually has three types of seasonality:
a daily pattern, a weekly pattern, and an annual pattern.

Such multiple seasonal patterns are becoming more common with high frequency data recording. Further examples where multiple where multiple seasonal patterns can
occur include call volume in call centers, daily hospital admissions, requests for cash at ATMs, electricity and water usage, and access to computer websites.

TO dela with such series, we will use the ```msts``` class which handles multiple seasonality time series. This allows you to specify all of the frequencies that
might be relevant. It is also flexible enough to handle non-integer frequencies.

Despite this flexibility, we don't necessarily want to include all of these frequencies - just the ones that are likely to be present in the data.
#### STL with multiple seasonal periods
The ```mstl( )``` function is a variation on ```stl( )``` designed to deal with multiple seasonality. It will return multiple seasonal components, as well as a trend
and remainder component.
```
calls %>% mstl( ) %>%
  autoplot( ) + xlab("Week")
```
The decomposition can also be used in forecasting, with each of the seasonal components forecast using a seasonal naïve method and the seasonally adjusted data
forecasting using ETS or some other method. The ```stlf( )``` function will do this automatically.
```
calls %>% stlf( ) %>%
  autoplot( ) + xlab("Week")
```
#### Dynamic harmonic regression with multiple seasonal periods
With multiple seasonalities, we can use Fourier terms. Because there are multiple seasonalities, we need to add Fourier terms for each seasonal period. In this case,
the seasonal periods are 169 and 845, so the Fourier terms are of the form

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Fourier%20terms.png)

The ```fourier( )``` function can generate these for you.

The total number of Fourier terms for each seasonal period have been chosen to minimize the AICc. We will use a log transformation (lambda=0) to ensure the forecasts
prediction intervals remain positive.

#### TBATS models
An alternative approach uses a combination of Fourier terms with an exponential smoothing state space model and a Box-Cox transformation, in a completely automated
manner. As with many automated modeling framework, there may be cases where it gives poor results, but it can be a useful approach in some circumstances.

A TBATS model differs from dynamic harmonic regression in that the seasonality is allowed to change slowly over time in a TBATS model, while harmonic regression
terms force the seasonal patterns to repeat periodically without changing. One drawback of TBATS models, however, is that they can be slow to estimate, especially
with long time series.
#### Complex seasonality with covariates
TBATS models do not allow for covariates, although they can be included in dynamic harmonic regression models. One commong application of such models is electricity
demand modeling.
