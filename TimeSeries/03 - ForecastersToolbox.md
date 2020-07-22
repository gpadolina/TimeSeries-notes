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
