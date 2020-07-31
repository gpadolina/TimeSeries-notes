## Time Series Regression Models

The basic concept is that we forecast the time series of interest y assuming that it has a linear relationship with other time series x.

The *forecast variable* y is sometimes also called the regressand, dependent or explained variable. The *predictor variables* x are sometimes also called the
regressors, independent or explanatory variables.
## The linear model
#### Simple linear regression
In the simplest case, the regression model allows for a linear relationship between the forecast variable y and a single predictor variable x. The coefficients
b0 and b1 denote the intercept and the slow of the line respectively. The intercept b0 represents the predicted value of y when x=0. The slope b1 represents
the average predicted change in y resulting from a one unit increase in x.

We can think of each observation yt as consisting of the systematic or explained part of the model and the random error, et. The error term does not imply a
mistake, but a deviation from the underlying straight line model. It captures naything that may affect yt other than xt.

The equation is estimated in R using the ```tslm( )``` function:
```
tslm(Consumption ~ Income, data=uschange)
```
#### Multiple linear regression
When there are two or more predictor variables, the model is called a multiple regression model. Each of the predictor variables must be numerical. The
coefficients b1,...,bk measure the effect of each predictor after taking into account the effets of all the other predictors in the model. Thus, the coefficients
measure the marginal effects of the predictor variables.
#### Assumptions
When we use a linear regression model, we are implicitly making some assumptions about the variables.

First, we assume that the model is a reasonable approximation to reality; that is, the relationship between the forecast variable and the predictor variables
satisfies this linear equation.

Second, we make the following assumptions about the errors(e1,...,et)
* They have mean zero; otherwise the forecasts will be systematically biased.
* They are not autocorrelated; otherwise the forecasts will be inefficient, as there is more information in the data that can be exploited.
* They are unrelated to the predictor variables; otherwise there would be more information that should be included in the systematic part of the model.

It is also useful to have the errors being normally distributed with a constant variance in order to easily produce prediction intervals.

Another important assumption in the linear regression model is that each predictor x is not a random variable. With observational data including most data in
business and economics, it is not possible to control the value of x, we simply observe it. Hence we make this assumption.
## Least squares estimation
The least squares principle provides a way of choosing the coefficients effectively by minimising the sum of the squared errors. That is, we choose the values
of b0,b1,...,bk that minimise the equation.

This is called *least squares* estimation because it gives the least value for the sum of squared errors. Finding the best estimates of the coefficients is
often called "fitting" the model to the data, or sometimes "learning" or "training" the model.

The ```tslm( )``` function fits a linear regression model to time series data. It is similar to the ```lm( )``` function which is widely used for linear models,
but ```tslm( )``` provides additional facilities for handling time series.
#### Fitted values
Predictions of y can be obtained by using the estimated coefficients in the regression equation and setting the error term to zero.

Plugging  in the values of x1t,...,xkt for t=1,...,T returns predictions of yt within the training-sample, referred to as *fitted values*.
#### Goodness-of-fit
A common way to summarize how well a linear regression model fits the data is via the cofficient of determination or R^2. This can be calculated as the square
of the correlation between the observed y values and the prediction y values. It reflects the proportion of variation in the forecast variables that is
accounted for or explained by the regression model.

If the predictions are close to the actual values, we would expect R^2 to be close to 1. On the other hand, if the predictions are unrelated to the actual values,
then R^2=0 assuming there is an intercept. In all cases, R^2 lies between 0 and 1.

The R^2 value is used frequently, though often incorrectly, in forecasting. The value of R^2 will never decrease when adding an extra predictor to the model and
this can lead to overfitting. There are no set rules for what is a good R^2 value, and typical values of R^2 depend on the type of data used. Validating a model's
forecasting performance on the test data is much better than measuring the R^2 value on the training data.
#### Standard error of the regression
Another measure of how well the model has fitted the data is the standard deviation of the residuals, which is often known as the residual standard error. The
standard error will be used when generating prediction invervals.
## Evaluating the regression model
After selecting the regression variables and fitting a regression model, it is necessary to plot the residuals to check that the assumptions of the model have
been satisfied. There are a series of plots that should be produced in order to check different aspects of the fitted model and the underlying assumptions.
#### ACF plot of residuals
With time series data, it is highly likely that the value of a variable observed in the current time period will be similar to its value in the previous period,
or even the period before that, and so on. Therefore when fitting a regression model to time series data, it is common to find autocorrelation in the residuals.

Another useful test of autocorrelation in the residuals designed to take account for the regression model is the *Breusch-Godfrey* test, also referred to as the LM
(Lagrange Multiplier) test for serial correlation. It is used to test the joint hypothesis that there is no autocorrelation in the residuals up to a certain
specified order. A small p-value indicates there is significant autocorrelation remaining in the residuals.

The Breusch-Godfrey test is similat to the Ljung-Box test, but it is specifically designed for use with regression models.
#### Histogram of residuals
It is always a good idea to check whether the residuals are normally distributed. This is not essential for forecasting, but it does make the calculation of
prediction intervals much easier.

Using the ```checkresiduals( )``` function, we can obtain the useful residual diagnostics.
```
checkresiduals(fit.consMR)
````
#### Residual plots against predictions
We would expect the residuals to be randomly scattered without showing any systematic patterns. If these scatterplots show a pattern, then the relationship may
be nonlinear and the model will need to be modified accordingly.
#### Residual plots against fitted values
A plot of the residuals against the fitted values should also show no pattern. If a pattern is observed, there may be "heteroscedasticity" in the errors which means
that the variance of the residuals may not be constant. If this problem occurs, a trasnformation of the forecast variable such as a logarithm or square root may be
required.
#### Outliers and influential observations
Observations that take extreme values compared to the majority of the data are called *outliers*. Observations that have a large influence on the estimated
coefficients of a regression model are called *influential observations.* Usually, influential observations are also outliers that are extreme in the x direction.

A scatter plot of y against each x is always a useful starting point in regression analysis, and often helps to identify unusual observations.

One source of outliers is incorrect data entry. Simple descriptive statistics of your data can identify minima and maxima that are not sensible. If such observation
is identified, and it has been recorded incorrectly, it should be corrected or removed from the sample immediately.

Outliers also occur when some observations are simply different. In this case it may not be wise for these observations to be removed. If an observation has been
identified as a likely outlier, it is important to study it and analyse the possible reasons behind it.
#### Spurious regression
More often that not, time series data are "non-stationary"; that is, the value of time series do not fluctuate around a constant mean or with a constant variance.

Regression non-stationary time series can lead to spurious regressions. High R^2 and high residual correlation can be signs of spurious regression.

## Some useful predictors
There are several predictors that occur frequently when using regression for time series data.
#### Trend
It is common for time series data to be trending. A trend variable can be specified in the ```tslm( )``` function using the ```trend``` predictor.
#### Dummy variables
What about when a predictor is a categorical variable taking only two values such as yes and no? Such variable might arise when forecasting daily sales and you want
to take account of whether the day is a *public holiday* or not. So the predictor takes value "yes" on a public holiday and "no" otherwise.

This situation can still be handled within the framework of multiple regression models by creating a "dummy variable" which takes value of 1 corresponding to "yes"
and 0 corresponding to "no". A dummy variables is also known as an "indicator variable".

A dummy variable can also be used to account for an *outlier* in the data. Rather than omit the outlier, a dummy variable removes its effect. In this case, the dummy
variable takes value 1 for that observation and 0 everywhere else.

If there are more than two categories, then the variable can be coded using several dummy variables. ```tslm( )``` will automatically handle this case if you specify
a factor variable as predictor.
#### Seasonal dummy variables
Suppose that we are forecasting daily data and we want to account for the day of the week as a predictor.

Notice that only six dummy variables are needed to code seven categories. That is because the seventh category is captured by the intercept and is specified when
the dummy variables are all set to zero.

Many beginners will try to add a seventh dummy variable for the seventh category. this is known as the "dummy variable trap", because it will cause the regression to
fail. There will be one too many parameters to estimate when an intercept is also included. The general rule is to use one fewer dummy variables than categories. So
for quarterly data, use three dummy variables; for monthly data, use 11 dummy variable; and for daily data, use six dummy variables and so on.

The interpretation of each of the coefficients associated with the dummy variables is that it is a measure of the effect of that category relative to the omitted
category. The ```tslm( )``` function will automatically handle this situation if you specify the predictor ```season```.
#### Intervention variables
It if often necessary to model interventions that may have affected the variable to be forecast.

When the effect lasts only for one period, we use a "spike" variable. This is a dummy variable that takes value one in the period of the intervention and zero
elsewhere. A spike variable is equivalent to a dummy variable for handling an outlier.

Other interventions have an immediate and permanent effect. If an intervention causes a level shift (the value of the series changes suddenly and permanently from
the time of intervention), then we use a "step" variable. A step variable takes value zero before the intervention and one from the time of intervention onward.

Another form of permanent effect is a change of slope. Here the intervention is handled using a piecewise linear trend; a trend that bends at the time of
intervention and hence is nonliear.
#### Trading days
The number of tradings in a month can vary considerably and can have a substantial effect on sales data. To allow for this, the number of trading days in each month
can be included as a predictor.

For monthly or quarterly data, the ```bizdays( )``` function will compute the number of trading days in each period.
#### Distributed lags
It is often useful to included advertising expenditure as a predictor. However, since the effect of advertising can last beyond the actual campaign, we need to
include lagged values of advertising expenditure.
#### Easter
Easter differs from most holidays because it is not held on the same day each year and its effect can last for several days. In this case, a dummy variable can be
used with value one where the holiday falls in the particular time period and zero otherwise.

The ```easter( )``` function will compute the dummy variable for you.
#### Fourier series
An alternative to using seasonal dummy variables, especially for long seasonal periods, is to use Fourier terms. Jean-Baptiste Fourier was a French mathematician,
born in the 1700s, who showed that a series of sine and cosine terms of the right frequencies can approximate any periodic function. We can use them for seasonal
pattern.

With Fourier terms, we often need fewer predictors than with dummy variables, especially when m is large. This makes them useful for weekly data, for example, where
m=52. For short seasonal periods, there is little advantage using Fourier terms over seasonal dummy variables.

These Fourier terms are produced using the ```fourier( )``` function.

THe first argument to ```fourier( )``` allows it to identify the seasonal period m and the length of the predictors to return. The second argument ```K``` specifies
how many pairs of sin and cos terms to include. The maximum allowed is K=m/2 where m is the seasonal period. Because we have used the maximum here, the results are
identical to those obtained when using seasonal dummy variables.

If only the first two Fourier terms are used(x1t, x2t), the seasonal pattern will follow a simple sine wave. A regression model containing Fourier terms is often
called a *harmonic regression* because the successive Fourier terms represent harmonics of the first two Fourier terms.
## Selecting predictors
When there are many possible predictors, we need some strategy for selecting the best predictors to use in a regression model.

A common approach that is not recommended is to plot the forecast variable against a particular predictor and if there is no noticeable relationship, drop that
predictor from the model. This is invalid because it is not always possible to see the relationship from a scatterplot, especially when the effects of other predictors
have not been accounted.

Another common approach which is also invalid is to do a multiple linear regression on all of the predictor and disregard all variables whose p-values are greater
than 0.05. To start with, statistical significance does not always indicate predictive value. Even if forecasting is not the goal, this is not a good strategy because
p-values can be misleading when two or more predictors are correlated with each other.
