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
