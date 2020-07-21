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
