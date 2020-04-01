---
layout: post
title: Probability Theory - Quick Note
tags: [Probability, Bayesian ML, Quick Note]
color: "#581845"
author: wassim
---

Probability theory revolves around three basic probability concepts, two rules and one theorm.

Given two random variables $$X$$ and $$Y$$. Suppose that $$X$$ can take any of the values $$x_i$$ where $$i = 1,...,M$$ and $$Y$$ can take the any of the values $$y_j$$ where $$j = 1,...,L$$. Let N be the total number of trials in which we sample both $$X$$ and $$Y$$, and let the number of such trials in which $$X=x_i$$ and $$Y = y_j$$ be $$n_{ij}$$. Also, let the number of trials in which $$X$$ takes the value $$x_i$$ (irrespective of the value that $$Y$$ takes) be denoted by $$c_i$$, and similarly let the number of trials in which $$Y$$ takes the value $$y_j$$ be denoted by $$r_j$$. Then we can introduce the following:

### Joint Probability

The probability that $$X$$ will take the value $$x_i$$ and $$Y$$ will take the value $$y_j$$ is given by:

$$ p(X = x_i, Y = y_j) = \frac{n_{ij}}{N} $$

### Marginal Probability

The probability that $$X$$ takes the value $$x_i$$, irrespective of the value of $$Y$$, is given by:

$$ p(X = x_i) = \frac{c_{i}}{N} $$

### Conditional Probability

If we consider only those instances for which $$X = x_i$$, then the fraction of such instances for which $$Y = y_j$$ is called the conditional probability of $$Y = y_j$$ given $$X = x_i$$ and is given by:

$$ p(Y = y_j | X = x_i) = \frac{n_{ij}}{c_i} $$

### Sum Rule

Because $$c_i = \sum_{j} n_{ij}$$ as per the definitions above, we can redefine the _Marginal Probability_ and introduce the _Sum Rule_ as follows:

$$ p(X = x_i) = \sum_{j=1}^{L}{p(X = x_i, Y = y_j)} $$

### Product Rule

Given the definitions of the _Joint Probability_, the _Marginal Probability_ and the _Sum Rule_, we can redefine the _Joint Probability_ and introduce the _Product Rule_ as follows:

$$ p(X = x_i, Y = y_j) = p(Y = y_j | X = x_i) * p(X = x_i) $$

### Bayes Theorm

Given the definition of the _Product Rule_ and the _Joint Probability_, we get _Bayes Theorm_ as follows:

$$ p(X | Y) = \frac{p(Y | X) p(Y)}{p(X)} $$

And using the _Sum Rule_ for the denominator, the theorm can be written as follows:

$$ p(X | Y) = \frac{p(Y | X) p(Y)}{\sum_{Y}{p(X | Y)p(Y)}} $$

In general, _Bayes Theorm_ can be used to convert a prior probability into a posterior probability by incorporating the evidence provided by the observed data.

### Probability Densities

When talking about probability in the context of continuous variables, and given a random real-valued variable $$x$$, then the probability $$p(x)$$ that this variable will lie in the range $$(a,b)$$ is called _Probability Density_ and is given as follows:

$$ p(x \in (a,b)) = \int_a^b p(x) dx $$

The sum and product rules of probability, as well as Bayes’ theorem, apply equally to the case of probability densities.
