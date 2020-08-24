## Advanced forecasting methods

## Complex seasonality
So far, we have considered relatively simple seasonal pattern such as quarterly and monthly data. However, higher frequency time series often exhibit more
complicated seasonal patterns. For example, daily data may have a weekly pattern as well as an annual pattern. Hourly data usually has three types of seasonality:
a daily pattern, a weekly pattern, and an annual pattern.

Such multiple seasonal patterns are becoming more common with high frequency data recording. Further examples where multiple where multiple seasonal patterns can
occur include call volume in call centers, daily hospital admissions, requests for cash at ATMs, electricity and water usage, and access to computer websites.

TO dela with such series, we will use the ```msts``` class which handles multiple seasonality time series. This allows you to specify all of the frequencies that
might be relevant. It is also flexible enough to handle non-integer frequencies.

Despite this flexibility, we don't necessarily want to include all of these frequencies - just the ones that are likely to be present in the data.
#### STL with multiple seasonal periods
The ```mstl( )``` function is a variation on ```stl( )``` designed to deal with multiple seasonality. It will return multiple seasonal components, as well as a trend
and remainder component.
```
calls %>% mstl( ) %>%
  autoplot( ) + xlab("Week")
```
The decomposition can also be used in forecasting, with each of the seasonal components forecast using a seasonal naïve method and the seasonally adjusted data
forecasting using ETS or some other method. The ```stlf( )``` function will do this automatically.
```
calls %>% stlf( ) %>%
  autoplot( ) + xlab("Week")
```
#### Dynamic harmonic regression with multiple seasonal periods
With multiple seasonalities, we can use Fourier terms. Because there are multiple seasonalities, we need to add Fourier terms for each seasonal period. In this case,
the seasonal periods are 169 and 845, so the Fourier terms are of the form

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Fourier%20terms.png)

The ```fourier( )``` function can generate these for you.

The total number of Fourier terms for each seasonal period have been chosen to minimize the AICc. We will use a log transformation (lambda=0) to ensure the forecasts
prediction intervals remain positive.

#### TBATS models
An alternative approach uses a combination of Fourier terms with an exponential smoothing state space model and a Box-Cox transformation, in a completely automated
manner. As with many automated modeling framework, there may be cases where it gives poor results, but it can be a useful approach in some circumstances.

A TBATS model differs from dynamic harmonic regression in that the seasonality is allowed to change slowly over time in a TBATS model, while harmonic regression
terms force the seasonal patterns to repeat periodically without changing. One drawback of TBATS models, however, is that they can be slow to estimate, especially
with long time series.
#### Complex seasonality with covariates
TBATS models do not allow for covariates, although they can be included in dynamic harmonic regression models. One commong application of such models is electricity
demand modeling.
## Vector autoregressions
One limitation of the models that we have considered so far is that they impose an unidirectional relationship - the forecast variable is influenced by the predictor
variables, but not vice versa. However, there are many cases where the reverse should also be allowed for - where all variables affect each other.

Such feedback relationships are allowed for in the vector autoregressive (VAR) framework. In this framework, all variables are treated symmetrically. They are all
modeled as if they all influence each other equally. In more formal terminology, all variables are now treated as "endogenous."

A VAR model is a generalization of the univariate autoregressive model for forecasting a vector of time series.

If the series are stationary, we forecast them by fitting a VAR to the data directly known as a "VAR in levels." If the series are non-stationary, we take differences
of the data in order to make them stationary, then fit a VAR model known as "VAR in differences."

VAR models are implemented in the *vars package* in R. It contains a function ```VARselect( )``` for selecting the numbers of lags p using four different information
criteria: AIC, HQ, SC, and FPE. We have met the AIC before and SC is simply another name for the BIC, which stands for Schwartz Criterion. HQ is the Hannan-Quinn
criterion and FPE is the "Final Prediction Error" criterion. Care should be taken when using the AIC as it tends to choose large numbers of lags. Instead, for
VAR modes, we prefer to use the BIC.

VARS are useful in several contexts:
1. forecasting a collection of related variables where no explicit interpretation is required.
2. testing whether one variable is useful in forecasting another (the basis of Granger causality tests).
3. impulse response analysis, where the response of one variable to a sudden but temporary change in another variable is analyzed.
4. forecasts error variance decomposition, where the proportion of the forecast variance of each variable is attributed to the effects of the other variables.
## Neural network models
Artificial neural networks are forecasting methods that are based on simple mathematical models of the brain. They allow complex nonlinear relationships between the
response variable and its predictors.
#### Neural network architecture
A neural network can be thought of as a network of "neurons" which are organized in layers. The predictors or inputs form the bottom layer and the forecasts or
outputs form the top layer. There may also be intermediate layers containing hidden neurons.

The simplest networks contain no hidden layers and are equivalent to linear regressions. The figure below shows the neural network version of a linear regression
with four predictors. The coefficnets attached to these predictors are called weights. These forecasts are obtained by a linear combination of the inputs. The
weights are selected in the neural network frame using a learning algorithm that minimizes a cost function such as the MSE.

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Simple%20neural%20network%20equivalent%20to%20linear%20regression.png)

Once we add an intermediate layer with hidden neurons, the neural network becomes non-linear.

![equation](https://github.com/gpadolina/TimeSeries-notes/blob/master/TimeSeries/Equations/Neural%20network%20with%20four%20inputs%20and%20one%20hidden%20layer%20with%20three%20hidden%20neurons.png)

This is known as a *multilayer feed-forward network*, where each layer of nodes receives inputs from the previous layers. The outputs of the nodes in one layer
are inputs to the next layer. The inputs to each node are combined using a weighted linear combination. The result is then modified by a nonlinear function
before being output.

#### Neural network autoregression
With time series data, lagged values of the time series can be used as inputs to a neural network, just as we used lagged values in a linear autoregression model.
We call this a neural network autoregression or NNAR model.

We only consider feed-forward networks with one hidden layer and we use the notation NNAR(p,k) to indicate there are p lagged inputs and k nodes in the hidden layer.
For example, a NNAR(9,5) model is a neural network with the last nine observations(yt-1,yt-2,...,yt-9) used as inputs for forecasting the output yt and with five
neurons in the hidden layer.

The ```nnetar( )``` function fits an NNAR(p,P,k)m model. If the values of p and P are not specified, they are selected automatically. For non-seasonal time series,
the default is the optimal number of lags (according to the AIC) for a linear AR(p) model. For seasonal time series, the default values are P=1 and p is chosen
from the optimal linear model fitted to the seasonally adjusted data. If k is not specified, it is set to k = (p + P + 1)/2.

When it comes to forecasting, the network is applied iteratively. For forecasting one step ahead, we simply use the available historical inputs. For forecasting
two steaps ahead, we use the one-step forecast as an input, along with the historical data.
#### Prediction intervals
Unlike most of the methods, neural networks are not based on a well-defined stochastic model and so it is not straightforward to derive prediction intervals
for the resultant forecasts.

## Bootstrapping and bagging
#### Bootstrapping time series
We can generate new time series that are similar to our observed series, using another type of bootstrap.

First, the time series is Box-Cox transformed and then decomposed into trend, seasonal, and remainder components using STL. Then we obtained shuffled versions of
the remainder component to get boostrapped remainder series. Because there may be autocorrelation present in an STL remainder series, we cannot simply use the
re-draw procedure. Instead, we use a blocked bootstrap, where contiguous sections of the time series are selected at random and joined together. These bootstrapped
remainder series are added to the trend and seasonal components and the Box-Cox transformation is reverse to give variations on the original time series.

This type of bootstrapping can be useful in two ways. First, it helps us to get a better measure of forecast uncertainty and second, it provides a way of improving
our point forecasts using bagging.
#### Prediction intervals from bootstrapped series
Almost all prediction intervals from time series models are too narrow. This is a well-known phenomenon and arises because they do not account for all sources of
uncertainty. Hyndman et al measured the size of the problem by computing the actual coverage percentage of the prediction intervals on test data and found that
for ETS models, nominal 95% intervals may only provide coverage between 71% and 87%. The difference is due to missing sources of uncertainty.
