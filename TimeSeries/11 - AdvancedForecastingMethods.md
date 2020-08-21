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
