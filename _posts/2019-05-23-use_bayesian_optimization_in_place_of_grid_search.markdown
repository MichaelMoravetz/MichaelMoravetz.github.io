---
layout: post
title:      "Use Bayesian Optimization in place of Grid Search"
date:       2019-05-23 18:44:39 +0000
permalink:  use_bayesian_optimization_in_place_of_grid_search
---


Once a model has been fit, often one of the last steps is to tune the hyperparameters.  Traditionally, this is done using two methods. An exhaustive gridsearch(with crossvalidation) and/or a randomized gridsearch./ . This methods can achieve good results but are somewhat archaic in nature.  They can be very tim consumptive and are limited to searching all or part of the parameter serach space passed to the gridsearch object.  Bayesian Optimization is an up and coming approach to optimizing parametersin a more inteligent way.  Instead of randomly or exhaustively searching the parameters space.   Bayesian optimization can find good parameters for anything with a black box loss function that would otherwise be difficult to optimize.  Bayesian optimization uses a surrogat function to fit a set of parameters and evaluate the cost.  It then makes a decision for the next search space based on the evaluation of the previous parameter space.  In this way, optimal parameters can be found with much less computation than other optimization techniques.
 To implement Bayesian Optimization, I will use  the scikit-optimization module.  This blog will walk through the steps involved in that process.
 
 1. Explain Bayesian optimization
 2. Explore and explian skopt module, its classes and functions.
 3. Show examples of how it works

To be continued....
