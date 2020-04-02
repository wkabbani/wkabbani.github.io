---
layout: post
title: Bayesian Machine Learning - Why?!
tags: [Bayesian Machine Learning]
color: "#00a8cc"
author: wassim
---

To understand what _Bayesian Machine Learning_ is, and why we need a bayesian approach to machine learning, we need to understand the inherent shortcomings with the traditional approach to machine learning.

Let's first review some very basic concepts in machine learning.

## Basic Concepts

#### Generalization

The ability of a machine learning model to deal with new examples that differ from those used for training.

#### Preprocessing

The process of transforming the original input into some new space of variables where the pattern recognition problem could be easier to solve.

#### Supervised Learning

Applications where the training data, used to train the model, comprises the inputs and their corresponding outputs.

#### Classfication Task

A sub category of _Supervised Learning_ where the output is one of a finite number of discrete values.

#### Regression Task

A sub category of _Supervised Learning_ where the output is one or more continues values.

#### Unsupervised Learning

Applications where the training data, used to train the model, comprises only the inputs without their corresponding outputs.

#### Clustering Task

A sub category of _Unsupervised Learning_ where the goal is to discover groups of similar examples within the input data.

#### Density Estimation Task

A sub category of _Unsupervised Learning_ where the goal is to determine the distribution of data within the input space.

#### Visualization Task

A sub category of _Unsupervised Learning_ where the goal is to project the data from a high-dimensional space down to a low-dimensional space for the purpose of visualizing the data.

#### Reinforcement Learning

Applications which tackle the problem of finding suitable acations to take in a given situation in order to maximize a reward.

## What is the traditional approach to ML?

Let's consider a very simple regression problem of predicting the value of an output variable $$t$$ given an input variable $$x$$, and a training dataset $$X$$ of observations of the variable $$x$$, together with their corresponding observations $$T$$ of the variable $$t$$.

The traditional way to go about solving this kind of problems is a simple approach based on curve fitting. That is, we fit the training data set of $$X$$ and $$T$$ using a polynomial funtion. To do that we need to determine the values of the coefficients of the polynomial as well as the order of the polynomial.

Determinining the coefficients can be done by minimizing some _error function_ (eg. sum-of-squared errors, root-mean-square error), which is just a measure of how different the results we get from the polynomial to the actual values in the training dataset.

Determining the order of the polynomial can be done by using an other separate dataset for comparing different values for the order, then we choose the value that performs better.

## What is the problem with the traditional approach?

The main issue with the traditional approach is that it doesn't allow us to build complex models when the training dataset is small, which is actually the case in most real-world applications. Because when the training data set is small, complex models start to _over-fit_, which means they don't generalize well, and so they fail when faced with new real-world data.

One way to deal with the over-fitting problem is a technique called _Regularization_, which adds a penalty to the error function to discourage large values for the coefficients. However we still need a way to determine a suitable value for the model complexity, and using two datasets one for training and one just to optimize and determine the model complexity is not always possible or practical in real-world applications as mentioned.

So we need an approach that will allow us to develop models with a degree of complexity that suits the complexity of the problem they are trying to solve, and to do that with small amount of training data.
