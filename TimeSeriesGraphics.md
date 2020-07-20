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
