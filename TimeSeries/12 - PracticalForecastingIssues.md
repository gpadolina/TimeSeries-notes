## Practical forecasting issues

## Weekly, daily, and sub-daily data
Weekly, daily, and sub-daily data can be challenging for forecasting, although for different reasons.
#### Weekly data
Weekly data is difficult to work with because the seasonal time period (the number of weeks in a year) is both large and non-integer. The average number of weeks in
a year is 52.18. Most of the methods we have considered require the seasonal period to be an integer. Even if we approximate it by 52, most of the methods will not
handle such a large seasonal period efficiently.

The simplest approach is to use an STL decomposition along with a non-seasonal method applied to the seasonally adjusted data. For example, 
```
gasoline %>% stlf( ) %>% autoplot( )
```
