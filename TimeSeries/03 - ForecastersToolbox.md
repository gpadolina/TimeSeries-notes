## Simple forecasting methods

#### Average method
The forecasts of all future values are equal to the average (or "mean") of the historical data.
```
meanf(y, h)
# y contains the time series
# h is the forecast horizon
```
#### Naïve method
For naïve method, simply set all forecasts to be the value of the last observation. This method works remarkably well for many
economic and financial time series. Because a naïve forecast is optimal when data follow a random walk, these are also called
*random walk forecasts.*
```
naive(y, h)
rwf(y, h) # Equivalent alternative
```
#### Seasonal naïve method
A similar method is useful for highly seasonal data. In this case, set each forecast to be equal to the last observed value
from the same season of the year (eg., the same month of the previous year).
```
snaive(y, h)
```
#### Drift method
A variation on the naïve method is to allow the forecasts to increase or decrease over time, where the amount of change over
time (called the drift) is set to be the average change seen in the historical data.
```
rwf(y, h, drift=TRUE)
```
#### Examples
```
autoplot( ) +
  autolayer(meanf( ), series="Mean") +
  autolayer(naive( ), series="Naive") +
  autolayer(snaive( ), series="Seasonal naive") +
  ggtitle( ) +
  xlab( ) +
  ylab( ) +
  guides( )
```
Sometimes one of these simple methods will be the best forecasting method available; but in many cases, these methods
will serve as benchmarks rather than the method of choice.

## Transformations and adjustments
Adjusting the historical data can often lead to a simpler forecasting task. There are four kinds of adjustments: calendar 
adjustments, population adjustments, inflation adjustments, and mathematical transformations. The purpose of these 
adjustments and transformations is to simplify the patterns in the historical data by removing known sources of variation
or by making the pattern more consistent across the whole data set. Simpler patterns usually lead to more accurate 
forecasts.
#### Calendar adjustments
Some of the variation seen in seasonal data may be due to simple calendar effects. In such cases, it is usually much easier 
to remove the variation before fitting a forecasting model. The ```monthdays( )``` function will compute the number of 
days in each month or quarter.
#### Population adjustments
Any data that are affected by population changes can be adjusted to give per-capita data. That is, consider the data per
person (or per thousand people, or per million people) rather than the total. For most data that are affected by 
population changes, it is best to use per-capita data rather than the totals.
#### Inflation adjustments
Data which are affected by the value of money are best adjusted before modelling. For example, the average cost of a new
house will have increases over the last few decades due to inflation. A $200,000 house this year is not the same as a
$200,000 house twenty years ago. Price indexes are often constructed by government agencies. For consumer goods, a common
price index is the Consumer Price Index or CPI.
#### Mathematical transformations
If the data show variation that increases or decreases with the level of the series, then a transformation can be useful.
For example, a logarithmic transformation is often useful. Logarithms are useful because they are interpretable: changes
in a log value are relative (or percentage) changes on the original scale. Another useful feature of log transformations
is that they constrain the forecasts to stay positive on the original scale.

Sometimes other transformations are also used but not so interpretable. For example, square roots and cube roots can be
used. These are called *power transformations.*

A useful family of transformations, that includes both logarithms and power transformations, is the family of Box-cox
transformations, which depend on the parameter lambda. The logarithm in a Box-cox transformation is always a natural
logarithm or to base e.
#### Bias adjustments
One issue with using mathematical transformations such as Box-cox transformations is that the back-transformed point
forecasts will not be the mean of the forecast distribution. In fact, it will usually be the median of the forecast
distribution assuming that the distribution on the transformed space is symmetric. For many purposes, this is acceptable,
but occasionally the mean forecast is required.

The difference between the simple back-transformed forecast and the mean is called the *bias.* When we use the mean, rather
than the median, we say the point forecasts have been bias-adjusted. Bias adjustment is not done by default in the *forecast*
package. If you want your forecasts to be means rather than medians, use the argument ```biasadj=TRUE``` when you select
your Box-cox transformation parameter.

## Residual diagnostics

### Fitted values
Each observation in a time series can be forecast using all previous observations. These are called *fitted values*.
### Residuals
The residuals in a time series model are what is left over after fitting a model. For many time series models, the residuals
are equal to the difference between the observations and the corresponding fitted values. Residuals are useful in checking
whether a model has adequately captures the information in the data. A good forecasting method will yielf residuals with the
following properties: the residuals are uncorrelated; the residuals have zero mean. Any forecasting method that does not satisfy
properties can be improved. In addition to these essential properties, it is useful but not necessary for the residuals to also
have the following properties: the residuals have constant variance; the residuals are normally distributed.
#### Example
```
autoplot(timeseries) +
  xlab( ) + ylab( ) +
  ggtitle( )
  
res <- residuals(naive( ))
autoplot(res) + xlab( ) + ylab( ) +
  ggtitle( )

gghistogram(res) + ggtitle( )

ggAcf(res) + ggtitle( )
```
#### Portmanteau tests for autocorrelation
In addition to looking at the ACF plot, we can also do a more formal test for autocorrelation by considering a whole set of rk
values as a group, rather than treating each one separately. When we look at the ACF plot to see whether each spike is within
the required limits, we are implicitly carrying out multiple hypothesis tests, each one with a small probability of giving a false
positive.

A test for a group of autocorrelations is called a *portmanteau test*, from a French word describing a suitecase contraining a
numer of items. One such test is the *Box-Pierce test*. A related and more accurate test is the *Ljung-Box test*.

All of these methods for checking residuals are conveniently packaged into one R function ```checkresiduals( )```, which will
produce a time plot, ACF plot, and histogram of the residuals with an overlaid normal distribution for comparison, and do a
Ljung-Box test with the correct degrees of freedom.

## Evaluating forecast accuracy

#### Training and test sets
It is important to evaluate forecast accuracy using genuine forecasts. Consequently, the size of the residuals is not a reliable
indication of how large true forecast errors are likely to be. The accuracy of forecasts can only be determined by considering
how well a model performs on new data that were not used when fitting the model.

When choosing models, it is common practice to separate the available data into two portions, *training* and *test* data, where the
training data is used to estimate any parameters of a forecasting methods and the test data is used to evaluate its accuracy.

The size of the test set is typically about 20% of the total sample, although this value depends on how long the same is and how
far ahead you want to forecast. The test set should ideally be at least as large as the maximum forecast horizon required.
#### Functions to subset a time series
The ```window( )``` function is useful when extracting a portion of a time series, such as when creating training and test sets. 
```
window(timeseries, start=year)
```
Another useful function is ```subset( )``` which allows for more types of subsetting. A great advantage of this function is that it
allows the use of indices to choose a subset. It also allows extracting all values for a specific season.
```
subset(timeseries, start=length(timeseries))
```
```head``` and ```tail``` are useful for extracting the first few or last few observations.
```
tail(timeseries)
```
#### Forecast errors
A forecast error is the difference between an observed value and its forecast. Here, error does not mean a mistake, it means the
unpredictable part of an observation.

Note that forecast errors are different from residuals in two ways. First, residuals are calculated on the training set while
forecast errors are calculated on the test set. Second, residuals are based on one-step forecasts while forecast errors can
involve multi-step forecasts.
#### Scale-dependent errors
The forecast errors are on the same scale as the data. Accuracy measures that are based only on *et* are scaled-dependent and cannot be
used to make comparisons between series that involve different units. The two most commonly used scale-dependent measures are based on
the absolute erorrs or squared errors: mean absolute error and root mean squared error.

When comparing forecast methods applied to a single time series, or to several time series with the same units, the MAE is popular as it
is easy to both understand and compute. A forecast method that minimises the MAE will lead to forecasts of the median, while minimising
the RMSE will lead to forecasts of the mean.
#### Percentage errors
The percentage error is given by pt = 100et/yt. Percentage errors have the advantage of being unit-free and so are frequently used to
compared forecast performances between data sets. The most commonly used measure is mean absolute percentage error.

Another problem with percentage errors that is often overlooked is that they assume the unit of measurement has a meaningful zero. They
also have the disadvantage that they put a heavier penalty on negative errors than on positive errors.This observation led to the use of
the so-called symmetric MAPE (sMAPE). The value of sMAPE can be negative, so it is not really a measure of absolute percentage errors at all.
#### Scaled errors
Scaled errors are an alternative to using percentage errors when comparing forecast accuracy across series with different units. It was
proposed scaling the errors on the training MAE from a simple forecast method. For a non-seasonal time series, a useful way to define a
scaled error uses naïve forecasts.

A scaled error is less than one if it arises from a better forecast than the average naïve forecast computed on the training data.
Conversely, it is greater than one if the forecast is worse than the average naïve forecast computed on the training data.
#### Example
```
timeseries2 <- window(timeseries, start=, end=)
timeseries2a <- meanf(timeseries2)
timeseries2b <- rwf(timeseries2)
timeseries2c <- snaive(timeseries2)
autoplot(window(timeseries, start=)) + 
  autolayer(timeseries2a, series="Mean") +
  autolayer(timeseries2b, series="Naïve) +
  autolayer(timeseries2c, series="Seasonal naïve) +
  xlab( ) + ylab( ) +
  ggtitle( )
  
timeseries3 <- window(timeseries, start=)
accuracy(timeseries2a, timeseries3)
accuracy(timeseries2b, timeseries3)
accuracy(timeseries2c, timeseries3)
```
#### Time series cross-validation
A more sophisticated version of training/test sets is time series cross-validation. In this procedure, there are a series of test sets,
each consisting of a single observation. The corresponding training set consists only of observations that occurred *prior* to the 
observation that forms the test set. Thus, no future observations can be used in constructing the forecast.

The forecast accuracy is computed by averaging over the test sets. This procedure is sometimes known as "evaluation on a rolling forecasting
origin" because the "origin" at which the forecast is based rolls foreward in time.

Time series cross-validation is implemented with the ```tsCV( )``` function.

A good way to choose the best forecasting model is to find the model with the smallest RMSE computed using time series cross-validation.
#### Pipe operator
Nesting functions within functions within functions can be messy and the code needs to be read from the inside out, which makes it difficult
to understand what is being computed. Instead, use the pipe operator ```%>%``` as follow.
```
timeseries %>% tsCV(forecastfunction=rwl, drift=TRUE) -> e
e^2 %>% mean(na.rm=TRUE) %>% sqrt( )
```
The left hand side of each pipe is passed as the first argument to the function on the right hand side. This is consistent with the way we
read from left to right in English. When using pipes, all other arguments must be named, which also helps readability. When using pipes,
it is also natural to use the right arrow assignment ```->``` rather than the left arrow.

## Prediction intervals
A prediction interval gives an interval within which we expect yt to lie with a specified probability. The value of prediction intervals is
that they express the uncertainty in the forecasts. If we only produce point forecasts, there is no way of telling how accurate the
forecasts are. However, if we also produce prediction intervals, then it is clear how much uncertainty is associated with each forecast.
#### One-step prediction intervals
When forecasting one step ahead, the standard deviation of the forecast distribution is almost the same as the standard deviation of the
residuals. In fact, the two standard deviations are identical if there are no parameters to be estimated, as is the cast with the naïve
method. For forecasting methods involving paramters to be estimated, the standard deviation of the forecast distribution is slightly
larger than the residual standard deviation, although this difference is often ignored.
#### Multi-step prediction intervals
A common feature of prediction intervals is that they increase in length as the forecast horizon increases. The further ahead we forecast,
the more uncertainty is associated with the forecast, and thus the wider the prediction intervals.
#### Benchmark methods
* Mean forecast
* Naïve forecas
* Seasonal naïve forecast
* Drift forecast
#### Prediction intervals from bootstrapped residuals
When a normal distribution for the forecast errors is an unreasonable assumption, one alternative is to use bootstrapping, which only assumes that
the forecast errors are uncorrelated.
...
Doing this repeatedly, we obtain many possible futures. Then we can compute prediction intervals by calculating percentiles for each forecast
horizon. The result is called a *bootstrapped* prediction interval. The name "bootstrap" is a reference to pulling ourselves up by our bootstraps,
because the process allows us to measure future uncertainty by only using the historical data.

To generate such intervals, we can simply add the ```bootstrap``` argument to our forecasting functions.
#### Prediction intervals with transformations
If a transformation has been used, then the prediction interval should be computed on the transformed scale, and the end points back-transformed
to give a prediction interval on the original scale.

The back-transformation of prediction intervals is done automatically using the functions in the *forecast* package in R, provided you have used
the ```lambda``` argument when computing the forecasts.
## The forecast package in R

#### Functions that output a forecast object
Many functions, including ```meanf( )```, ```naive( )```, ```snaive( )```, and ```rwf( )``` produce output in the form of a ```forecast``` object.
This allows other functions such as ```autoplot( )``` to work consistently across a range of forecasting models.

Objects of a class ```forecast``` contain information about the forecasting method, the data used, the point forecasts obtained, prediction intervals,
residuals and fitted values. There are several functions designed to work with these objects including ```autoplot( )```, ```summary( )```, and
```print( )```.

The following list shows all the functions that produce ```forecast``` objects.
* ```meanf( )```
* ```naive( )```, ```snaive( )```
* ```rwf( )```
* ```croston( )```
* ```stlf( )```
* ```ses( )```
* ```holt( )```, ```hw( )```
* ```splinef( )```
* ```thetaf( )```
* ```forecast( )```
#### ```forecast( )``` function
The ```forecast( )``` function works with many different types of inputs. It generally takes a time series or time series model as its main argument,
and produces forecasts appropriately. It always returns objects of class ```forecast```.
```
forecast(timeseries, h=4)
```
