---
layout: post
title:      "Use Bayesian Optimization in place of Grid Search"
date:       2019-05-23 14:44:40 -0400
permalink:  use_bayesian_optimization_in_place_of_grid_search
---

*A Laymans Understanding of a Bayesian Approach to Hyper Parameter Tuning in Python*

After feature engineering, transforming data, and fitting a model, the last step is often optimizing hyper parameters .  Traditionally, this is done using two methods;  a randomized search of parameter space or an exhaustive grid search of a given parameter space. These methods can achieve good results but are somewhat archaic in nature.  They can be very time consumptive and are limited to searching all or part of the parameter space provided with no intelligent choice of the next parameters to fit to a model.  Generally using cross validation is the only way to be certain of which parameters perfromed the best.  Doing a thorough search of all possible paramters could literally take days, especially for certain algorithms which are notoriously slow to fit.  This often leads to making the best guesses and limiting search space to what prior knowledge we may think we have.  Intuitively, this approach does not give a lot of confidence that the results are the best possible.  If you are anything like myself, you will always wonder, what if I changed this or that.  However, in reality we often just dont have time to commit to exhausting every possiblilty to be certain of our results.  This is where Bayesian optimization can step in and come to the rescue.

Bayesian Optimization is an up and coming approach to optimizing parameters in a more inteligent way.  Instead of randomly or exhaustively searching the parameters space,   Bayesian optimization can find ideal parameters for anything with a black box loss function that would otherwise be difficult to optimize.  Bayesian optimization uses a surrogate function to fit a set of parameters and evaluate the cost.  It then makes a decision for the next search space based on the evaluation of the previous parameter space.  In this way, optimal parameters can be found with much less computation than other optimization techniques.  Given enough time, bayesian optimization whould theoretically be able to find the best parameters, but from most my experience, with limited time, it will definitely get better results than the other grid search options, without luck being a factor.

## What is Bayesian Optimization?

From Cornell University's tutorial on Bayesian Optimization:
>Bayesian optimization is an approach to optimizing objective functions that take a long time (minutes or hours) to evaluate. It is best-suited for optimization over continuous domains of less than 20 dimensions, and tolerates stochastic noise in function evaluations. It builds a surrogate for the objective and quantifies the uncertainty in that surrogate using a Bayesian machine learning technique, Gaussian process regression, and then uses an acquisition function defined from this surrogate to decide where to sample.

source : [Cornell University's tutorial on Bayesian Optimization](http://arxiv.org/abs/1807.02811)

Let's break down some of the terms above to get a better grasp of what this is saying...

### Defining Terms
- **surrogate** : This term refers to *surrogate models* , these are used when the outcome of interest cannot be easily or directly measured, in which case a model of the outcome is used instead.  These *surrogate models* are constructed from a data driven, bottom up approach meaning an understanding of the inner workings of the model they are simluating do not need to be known.  They are constructed based on the response of the simulated model to a limited number of intelligently chosen data points. *Surrogate models*, also known as *behavioral modeling* or *black-box modeling*, mimic the behavior of the model being optimized as closely as possible but are much computationally cheaper to evaluate. 

- **objective function** : In linear programming is the function used for minimizing or maximizing. This is what we often call the cost or loss function. ie. Logloss, or in more general examples;  allocating machinery and labor between different products so that profit is maximized or costs are minimized. Or, managing truck fleets and determining the most profitable routes.

- **acquisition function** : This is the function computed from the surrogate and is used to guide the selection of the next evaluation point.   This can be different depending on the problem at hand, however a standard acquisiton function used is the *Expected Criterion Improvement* (EI).  This function provides the expected improvement of value of the function at the current evaluation point over the best value of the function yet seen.  Other examples of acquistion functions are *Entropy Search, Upper Confidence Bounds*, *Expected Loss Criterion*, and *knowledge gradient*.

- **stochastic noise** : Stochastic refers to  a randomly determined proccess. Particulary, where a random process is used to represent sytems  or phenomna that seem to change in a random way.  Noise refers to the stochastic properties that we do not know everything about.  This is often expressed in large fluctuations in output values and necesitates using a statistical model that can be known in order to predict the properties we are trying to optimize.  

- **Gausian Process** : Another stochastic process,  refers to a random collection of variables which in any finite collection of those variables will have a mulitvariate normal distribution (a generalization of one dimensional normal distribution to higer dimensions). Meaning every finite linear combination of the variables will be noirmally distributed.  An algorithm using a Gaussian process measures the similarity between between points to determine to predict the value of an unforseen point in the training data.  Gaussian process regression is used to fit the best values in the parameter space to the mulivariate distribution and not only provides information on the likely value of the function, but also provides information on the uncertainty around that value.

## Visualizing The Process

Take a look at the following plot provided by scikit-optimization in their documentation.   The dotted red line represents the true unknown function which we are trying to match.   The dotted green line represents the approximated of the orignial function by the Gaussian process model.   The shaded areas resprents how sure the Gaussian process in about the values at each point.  The red dots are observations evaluated throughout the optimizatioin process.

![](https://scikit-optimize.github.io/notebooks/bayesian-optimization_files/bayesian-optimization_21_0.png)

Looking at the plot above, you will see that the results are not perfect, however in certain places, where there were a number of observations, the approximated function is very close to the unknown function.  This is because throughout observatioins as the model is evaluated with different parameter values, the peaks and valleys of the gp are explored until best values are found. It then moves onto the next peak or value it beleives will show the best results,  a convergence plot of this proccess will show large drops in the objective function followed by a plateau for a number of observations, Those plateaus represetnt the points where the optimizatioin is making certain it is closing in on the best values.  Once those have been found, it moves onto the next area and repeats the proccess. So, given enough time, it will find best results with more confidence accross the whole search space.  If we were to compare this to a random grid search, you might see decent results with some parameters, but generally even Bayesain optimization performed for a limited number of iterations, where it doesnt perfectly optimize, will achieve much better results than grid searching in much less time, making Bayesian optimization a much more reliable option for finding better parameters with limited computational resources.

## Performing Bayesian Optimization in Python
To implement Bayesian Optimization, I use the Scikit-Optimize (skopt) module.  It is relatively user friendly, out of the box has good defaults, and allows for customizing parameters to meet the needs of any problem.  I recommend getting familiar with the various tools provided in the module. There are a number of functions and classes provided which can be used for defining your own classes, inheriting from their optimization class for your personal needs. Here is an example of how to perfrom Bayesian optimization with out of the box tools, it has been provided by skopt in their documentation...

##### Install skopt
This can be done in the terminal using pip or in a Jupyter notebook. The following example will show how it is done in a notebook.

```
!pip install sck-kit optimize
```

##### Load Data, Estimators and Evaluation Metrics

```
#Import dataset and tools

from sklearn.datasets import load_boston
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.model_selection import cross_val_score

# Load dataset, instantiate predictors and target variables.

boston = load_boston()
X, y = boston.data, boston.target
n_features = X.shape[1]

# Instantiate estimator class object and set any parameters desired before optimization
reg = GradientBoostingRegressor(n_estimators=50, random_state=0)
```

##### Define Parameter Search Space

Skopt provides a usefull class for this task. `skopt.space`. Objects from this class can be `Real` (floats), `Integer`,  or `Categorical`. This is recommended for ease of use and allows for setting limits of the search space as well as the name of the parameter.

```
from skopt.space import Real, Integer

# Define search spaces. Pass type (Real, Integer or Categorical), bounds, how to sample, and name parameter.

space  = [Integer(1, 5, name='max_depth'),
          Real(10**-5, 10**0, "log-uniform", name='learning_rate'),
          Integer(1, n_features, name='max_features'),
          Integer(2, 100, name='min_samples_split'),
          Integer(1, 100, name='min_samples_leaf')]
```

##### Define Objective Function
This is a step that requires a little thinking, as always choosing your best metric is important and it is a good idea to use cross validation.  There could be more simple ways of doing this, but taking the time to think it through will unsure confidence in you results. For tuning the hyperparameters of a sklearn estimator this can easily be done using a decorator and `use_named_args` for `skopt.utils`.   It allows the objective function to recieve the parameters to be optimized as keyword arguments for optimization. 

```
from skopt.utils import use_named_args

# Defining objective function

@use_named_args(space)  # Use this for accessing parameters to tune
def objective(**params):
    reg.set_params(**params)
    return -np.mean(cross_val_score(reg, X, y, cv=5, n_jobs=-1,
                                      scoring="neg_mean_absolute_error"))
```

##### Optimize paramters.
This can be done one at a time, or all at once.  I have found that doing one or two at a time, may help achieve slightly better scores, but it will take quite a bit longer. For this example, all paramters will be optimized in one fell swoop.  The `gp_minimize` function will be used and is a recommended good place to start for many problems, but keep in mind that ohters can be used.

```
from skopt import gp_minimize

# Call minimization function, Pass objective function, and set n_calls
# n_calls is the number of times the model will be evaluated witht he parameters

res_gp = gp_minimize(objective, space, n_calls=50, random_state=0)

# check best score

"Best score=%.4f" % res_gp.fun

```

```
>>>'Best score=2.9241'
```

```
# View the results of the optimization process

print("""Best parameters:
- max_depth=%d
- learning_rate=%.6f
- max_features=%d
- min_samples_split=%d
- min_samples_leaf=%d""" % (res_gp.x[0], res_gp.x[1], 
                            res_gp.x[2], res_gp.x[3], 
                            res_gp.x[4]))
```

```
>>> Best parameters:
>>> - max_depth=5
>>> - learning_rate=0.257228
>>> - max_features=13
>>> - min_samples_split=100
>>> - min_samples_leaf=1
```

##### Visualize The Optimization Process
You can easily plot a convergence plot using `skopt.plots.plot_convergence` which will show the objective function score at each evaluation of the model. As discussed above, note how the line flattens out while optimizing high points of the gp.
```
from skopt.plots import plot_convergence

plot_convergence(res_gp)
plt.show()
```

![](https://scikit-optimize.github.io/notebooks/hyperparameter-optimization_files/hyperparameter-optimization_13_1.png)

## Wrapping Up
There you have a quick explanation and tutorial on Bayesian optimization and how to implement it using a popular python module.  Getting familiar with all of the aspects of skopt can make creating your own optimization tools and defining your own functions a very useful and powerful tool to keep in the toolbox.  Good luck with your endeavors!
