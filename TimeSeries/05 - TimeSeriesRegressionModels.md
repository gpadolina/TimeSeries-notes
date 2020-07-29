## Time Series Regression Models

The basic concept is that we forecast the time series of interest y assuming that it has a linear relationship with other time series x.

The *forecast variable* y is sometimes also called the regressand, dependent or explained variable. The *predictor variables* x are sometimes also called the
regressors, independent or explanatory variables.
## Simple linear regression
In the simplest case, the regression model allows for a linear relationship between the forecast variable y and a single predictor variable x. The coefficients
b0 and b1 denote the intercept and the slow of the line respectively. The intercept b0 represents the predicted value of y when x=0. The slope b1 represents
the average predicted change in y resulting from a one unit increase in x.

We can think of each observation yt as consisting of the systematic or explained part of the model and the random error, et. The error term does not imply a
mistake, but a deviation from the underlying straight line model. It captures naything that may affect yt other than xt.
