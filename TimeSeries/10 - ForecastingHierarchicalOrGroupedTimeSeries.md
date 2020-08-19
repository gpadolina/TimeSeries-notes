## Forecasting hierarchical or grouped time series
Time series can often be naturally dissaggregated by various attributes of interest. For example, the total number of bicycles sold by a cycling manufacturer
can be disaggregated by product type such as road bikes, mountain bikes, children's bikes and hybrids. Each of these can be disaggregated into finer categories.
For example, hybrid bikes can be divided into city, commuting, comfort, and trekking bikes. These categories are nested within the larger group categories and
so collection of time series follow a hierarchical aggregation structure. Therefore, we refer to these as "hierarchical time series".

Hierarchical time series often arise due to geographic divisions. For example, the total bicycle sales can be disaggregated by country, then within each country
by state, within each state by region, and so on down to the outlet level.

Our bicycle manufacturer may disaggregate sales by both product type and by geographic location. Then we have a more complicated aggregation structure where the
product hierarchy and the geographic hierarchy can be both used together. We usually refer to these as "grouped time series".

It is common to produce disaggregated forecasts based on disaggregated time series and we usually require the forecasts to add up in the same way as the data.
For example, forecasts of regional sales should add up to give forecasts of state sales, which should in turn add up to give a forecast for the national sales.

We discuss forecasting large collection of time series that must add up in some way. The challenge is that we require forecasts that are *coherent* across the
aggregation structure. That is, we require forecasts to add up in a manner that is consistent with the aggregation strucutre of the collection of time series.
## Hierarchical time series
Figure below shows a K = 2-level hiearchical structure. At the top of the hierarchy (which we clal level 0) is the "Total", the most aggregate level of the data.
The *t*-th observation of the Total series is denoted by yt for t = 1,...,T. The total is disaggregated into two series at level 1, which in turn are divided
into three and two series respectively at bottom-level of the hierarchy.

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Two-level%20hierarchical%20tree%20diagram.png)

In this example, the total number of series in the hierarchy is n = 1 + 2 + 5 = 8, while the number of series at the bottom-level is m = 5. Note that n > m in all
hierarchies.

For the hierarchical structure above, we can write

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Hierarchical%20structure%20equation.png)

## Grouped time series
Grouped time series involve more general aggregation structures than hierarchical time series. With grouped time series, the structure does not naturally disaggregate
in a unique hierarchical manner and often the disaggregating factors are both nested and crossed.

The figure below shows a K = 2-level grouped structure. At the top of the grouped structure is the Total, the most aggregate level of the data, again represented by yt.
The Total can be disaggregated by attributed (A, B) forming series yA,t and yB, or attributes (X, Y) forming series yX,t and yY,t. At the bottom level, the data are
disaggregated by both attributes.

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Two-level%20grouped%20structure.png)

Grouped time series can be sometimes thought of as hierarchical time series that do not impose a unique hierarchical structure, in the sense that the order by which
the series can grouped is not unique.
## The bottom-up approach
A simple method for generating coherent forecasts is the bottom-up approach. This approach involves first generating forecasts for each series at the bottom-level
and then summing these to produce forecasts for all the series in the structure.

An advantage of this approach is that we are forecasting at the bottom-level of a structure and therefore no information is lost due to aggregation. On the other
hand, bottom-level data can be quite noisy and more challenging to model and forecast.

#### The hts package for R
Forecast can be produced using the ```forecast( )``` function applied to objects created by ```hts( )``` or ```gts( )```. The hts package has thee built-in options
to produce forecasts: ETS models, ARIMA models or random walks; these are controlled by the ```fmethod``` argument. It also use several methods for producing
coherent forecasts, controlled by the ```method``` argument.

For example,suppose we wanted bottom-up forecasts using ARIMA models applied to the prison data. Then we would use
```
forecast(timeseries, method="bu", fmethod="arima"
```
which will apply the ```auto.arima( )``` function to every bottom-level series in our collection of time series. Similarly, ETS models would be used if
```fmethod="ets"``` was used.
## Top-down approaches
Top-down approaches only work with strictly hierarchical aggregation structures and not with grouped structures. They involve first generating forecasts for the
Total series yt and then disaggregating these down the hierarchy.

The two most common top-down approaches specify disaggregation proportions based on the historical proportions of the data.
#### Average historical proportions

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Average%20historical%20proportions.png)

This approach is implemented in the ```forecast( )``` function by setting ```method="tdgsa"```, where ```tdgsa``` stands for "top-down Gross-Sohl method A".

#### Proportions of the historical averages

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Proportions%20of%20the%20historical%20averages.png)

This approach is implemented in the ```forecast( )``` function by setting ```method="tdgsf"```, where ```tdgsf``` stands for "top-down Gross-Sohl method F".

A convenient attribute of such top-down approaches is their simplicity. One only needs to model and generate forecasts for the most aggregated top-level series.
In general, these approaches seems to produce quiet reliable forecasts for the aggregate levels and they are useful with low count data. On the other hand,
one disadvantage is the loss of information due to aggregation. Using such top-down approaches, we are unable to capture and take advantage of individual series
characteristics such as time dynamics, special events, etc.

#### Forecast proportions
Because historical proportions used for disaggregation do not take account of how those proportions may change over time, top-down approaches based on historical
proportions tend to produce less accurate forecasts at lower levels of the hierarchy than bottom-up approaches. To address this issues, proportions bases on forecasts
rathen than historical data can be used.
