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
