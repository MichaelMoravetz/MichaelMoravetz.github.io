---
layout: post
title:      "Creating a Stacking Classifier using Sklearn API"
date:       2019-05-23 14:35:00 -0400
permalink:  creating_a_stacked_classifier_class
---


Stacked classifiers are all the rage in kaggle competetions.  Very few are won without ensembling multiple classifiers to obtain better scores.  This blog post will cover my approach to writing a python class that works with Scikit Learns API and can stack multiple classifiers.  It will put a series of base classifiers which will predict labels or probabilty and then pass those predictions to a metaclassifier which will be trained on the predictions of the base classifiers. With this approach,  theorietically the stacked classifier will be able to learn when the base classifiers are predicting wrong labels and correct those in the final prediction made by the MetaClassifier.

Here are the steps:
1. Get familiar with sklearn API
2. Define an estimator built on sklearn ClassifierMixin andTransformerMixin class to inherit for bas estimator classes. 
3. Create an estimator class with the necesary methods and attributes for it to work with sklearn's API.
4. Use sklearn Check_Estimator function to validate that your custom classifier works within the sklearn framework.


To be continued...

