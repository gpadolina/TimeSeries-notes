The first thing to do in any data analysis task is to plot the data. Graphs enable many features of the data to be visualized,
including patterns, unusual observations, changes over time, and relationships between variables.

### ```ts``` objects
A time series can be thought of as a list of numbers, along with some information about what times those numbers were
recorded. This information can be stored as a ```ts``` object in R.
```
y <- ts(c(100, 200, 300, 400, 500), start=2010)

or

y <- ts(z, start=2010, frequency=10)
```
#### Frequency of a time series
The frequency is the number of observations before the seasonal pattern repeats. When using the ```ts( )``` function in R, the
following choices are annual, quarterly, monthly, and weekly.

### Time plots
For time series data, the obvious graph to start with is a time plot. That observations are plotted against the time of observation,
with consecutive observations joined by straight lines.
```
autoplot(timeseries) +
  ggtitle(" ") +
  ylab(" ") +
  xlab(" ")
```
```Autoplot( )``` automatically produces an appropriate plot of whatever you pass to it in the first argument.

### Time series patterns
#### Trend
A trend exists when there is a long-term increase or decrease in the data. It does not have to be linear. Sometimes trend is
refered to as "changing direction", when it might go from an increasing trend to a decreasing trend.
#### Seasonal
A seasonal pattern occurs when a time series is affected by seasonal factors such as the time of the year or the day of the
week. Seasonality is always of a fixed and known frequency.
#### Cyclic
A cycle occurs when the data exhibit rises and falls that are not of a fixed frequency. These fluctuations are usually due
to economic conditions, and are often related to the "business cycle." The duration of these fluctuations is usually at least
2 years.
