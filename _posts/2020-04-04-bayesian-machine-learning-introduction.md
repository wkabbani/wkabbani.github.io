---
layout: post
title: Bayesian Machine Learning - Introduction
tags: [Bayesian Machine Learning, Pattern Recognition]
color: "#00a8cc"
author: wassim
---

To start thinking about bayesian machine learning, we first need to revisit two important concepts _bayesian probabilities_ and _Guassian distribution_.

## Bayesian Probabilities

In certain types of events that can't repeat multiple times, the _classical_ or _frequentist_ interpretation of probability doesn't really make much sense. That is why we need to consider the more general _Bayesian_ interpretation which views probabilities as quantifications of uncertainty.

The way bayesian probabilities work is _informally_ described as follows:

1. We capture our assumptions about the parameters $$w$$, which we would like to determine their values and before observing any data, in the form of a prior probability distribution $$p(w)$$.

2. We express the effect of observed data $$D$$ in the form of the conditional probability $$ p (D\|w) $$, and we call this the _**likelihood function**_ which expresses how probable the observed data set is for different settings of $$w$$.

3. We evaluate the uncertainty in $$w$$ after we have observed $$D$$ in the form of the posterior probability $$ p(w\|D) $$.

Considering our knowledge about [Bayes’ theorem](https://en.wikipedia.org/wiki/Bayes%27_theorem){:target="\_blank"}, we can rewrite the above as follows:

$$posterior ∝ likelihood × prior$$

## The Gaussian Distribution

There are many probability distributions. And one of the most important distributions for continuous variables is the _Gaussian_ or the _Normal_ distribution. Talking about this distribution here will help us introduce a number of important definitions and key concepts.

For the case of a single real-valued variable $$ x $$, the _Gaussian_ distribution is given by:

$$ N(x | \mu, \sigma^2) = \frac{1}{(2 \pi \sigma^2)^{1/2}} exp \lbrace - \frac{1}{2\sigma^2} (x - \mu)^2 \rbrace $$

### Definitions

- $$ \mu $$: is called the **_mean_**.
- $$ \sigma^2 $$: is called the **_variance_**.
- $$ \sigma $$: is called the **_standard deviation_**.
- $$ \beta = \frac{1}{\sigma^2}$$: is called the **_precision_**.
- The maximum of a distribution is called the **_mode_**.
- Data points that are drawn independently from the same distribution are said to be **_independent and identically distributed_** or **_i.i.d._**.
- The mean of the observed data is called the **_sample mean_**.
- The variance of the observed data is called the **_sample variance_**.

## Bayesian Machine Learning

Bayesian machine learning utilizes many concepts from various mathematical disciplines, like [_Probability Theory_](https://wkabbani.github.io/2020/03/31/probability-theory.html){:target="\_blank"} , _Decision Theory_, and _Information Theory_. Here we'll only talk about the bayesian machine learning and its relation to _Probability Theory_.

### Bayesian Machine Learning & Probabilities

So what does bayesian machine learning has to do with probabilities?

Due to the inherent uncertainty in the field of machine learning which arises from many sources like the limited size of training data or the different kinds of noise in the input data, we need a robust mean of manipulating and quantifing this uncertainty. And this mean is provided by probablity theory.

More concretely, the way bayesian machine learning and probability theory relate to each other, is that, we can use the machinery of probability theory to describe the uncertainty in model parameters, or in the choice of the model itself.

### The Likelihood Function

We will rewrite the likelihood function $$ p (D\|w) $$ mentioned earlier in terms of observations drawn independently from a gaussian distribution.

Given a data set $$ D $$ representing $$ N $$ observations of the scalar variable $$ x $$. And suppose the observations are drawn independently from a _Gaussian_ distribution whose mean $$ \mu $$ and variance $$ \sigma^2 $$ (which we don't know and we would like to determine using the observations).

Because the data set is **_i.i.d._**, and because we know from the definition of joint probability that the joint probability of two independent events is given by the product of the marginal probabilities for each event separately. Then the probability of the data set, given the variables $$ \mu $$ and $$ \sigma^2 $$ (i.e. the likelihood function) can be written as follows:

$$ p(D | \mu, \sigma^2) = \prod_{n=1}^{N} N(x_n | \mu, \sigma^2) $$

In both the Bayesian and frequentist paradigms, the likelihood function plays a central role. However, the manner in which it is used is fundamentally different in the two approaches.

In a frequentist setting, $$w$$ (here $$ \mu $$ and $$ \sigma^2 $$) is considered to be a fixed parameter, whose value is determined by some form of _estimator_. And a widely used frequentist estimator is _**maximum likelihood**_, in which $$w$$ is set to the value that maximizes the likelihood function.

In a Bayesian setting, there is only a single data set $$D$$, and the uncertainty in the parameters is expressed through a probability distribution over $$w$$.

One advantage of the Bayesian viewpoint is that the inclusion of prior knowledge arises naturally. Suppose, for instance, that a fair-looking coin is tossed three times and lands heads each time. A classical maximum likelihood estimate of the probability of landing heads would give 1, implying that all future tosses will land heads! By contrast, a Bayesian approach with any reasonable prior will lead to a much less extreme conclusion.

### The Maximum Likelihood

One widely used frequentist estimator, as we said, is the _**maximum likelihood**_. And the way it determines the parameters in a probability distribution (i.e. $$ \mu $$ and $$ \sigma^2 $$) using an observed data set is by finding the parameter values that maximize the likelihood function.

In practice, it's more convenient to maximize the log of the likelihood function. Because the logarithm is a monotonically increasing function of its arguments, and the maximization of the log of a function is equivalent to the maximization of the function itself.

One of the significant limitations of the maximum likelihood approach, is that it systematically underestimates the variance of the distribution, which is an example of a phenomenon called **_bias_** and is related to the problem of **_over-fitting_**. And this happens because the variance is measured relative to the sample mean and not relative to the true mean.

Also we shoud note that the bias of the maximum likelihood solution becomes less significant as the number $$N$$ of data points increases. And in practice, for anything other than small $$N$$, this bias will not prove to be a serious problem. However, for more complex models with many parameters, the bias problems associated with maximum likelihood will be much more severe. And In fact, the issue of bias in maximum likelihood lies at the root of the over-fitting problem.

## Bayesian Curve Fitting

Given a same simple regression problem, same one mentioned in [Bayesian Machine Learning - Why?!](https://wkabbani.github.io/2020/04/02/why-bayesian-machine-learning.html), with the goal of being able to make predictions for the target variable $$t$$ given some new value of the input variable $$x$$ on the basis of a set of training data comprising $$N$$ input values $$X = (x_1 , . . . , x_N )^T$$ and their corresponding target values $$T = (t_1 , . . . , t_N)^T $$.

We can see a bayesian approach to solve the problem outlined as follows:

### First Step

We start by expressing our uncertainty over the value of the target variable using a probability distribution. And we assume that, given the value of $$x$$, the corresponding value of $$t$$ has a _Gaussian_ distribution. Thus we get:

$$ p (t | x, w , \beta) = N (t | y(x,w), \beta^{-1}) $$

Now we use the training data $$ \{X,T\} $$ to determine the values of the unknown parameters $$w$$ and $$\beta$$.

### Second Step

We introduce a prior distribution over the parameters $$w$$. And using Bayes theorm about the posterior probability distribution, we can determine $$w$$ by finding the most probable value of $$w$$ given the data, in other words by maximizing the posterior distribution (as opposed to using the maximum likelihood).

### Third Step

We consistently apply the sum and product rules of probability, which requires, that we integrate over all values of $$w$$. And which allows the predictive distribution to be written in the form:

$$ p(t | x, X, T) = \int p(t | x, w) p(w | X, T) dw $$

where $$p(t \| x, w)$$ is given by the equation in step one, and $$ p(w \| X, T) $$ is the posterior distribution over the parameters.

And because now we have a probabilistic model, these predictions are expressed in terms of a **_predictive distribution_** which gives the probability distribution over the target value $$t$$, rather than simply a point estimate.

> The practical application of Bayesian methods was for a long time severely limited by the difficulties in carrying through the full Bayesian procedure, particularly the need to marginalize (sum or integrate) over the whole of parameter space, which is required in order to make predictions or to compare different models. But The development of sampling methods, such as Markov chain Monte Carlo along with dramatic improvements in the speed and memory capacity of computers, opened the door to the practical use of Bayesian techniques in an impressive range of problem domains. Monte Carlo methods are very flexible and can be applied to a wide range of models. However, they are computationally intensive and have mainly been used for small-scale problems. More recently, highly efficient deterministic approximation schemes such as [_variational Bayes_](https://en.wikipedia.org/wiki/Variational_Bayesian_methods) and [_expectation propagation_](https://en.wikipedia.org/wiki/Expectation_propagation) have been developed. These offer a complementary alternative to sampling methods and have allowed Bayesian techniques to be used in large-scale applications.

---

> Disclaimer: All the content is inspired by the great book [Pattern Recognition and Machine Learning, by Christopher M. Bishop](http://users.isr.ist.utl.pt/~wurmd/Livros/school/Bishop%20-%20Pattern%20Recognition%20And%20Machine%20Learning%20-%20Springer%20%202006.pdf). And all the credit is to be given to the author of the book [Christopher Bishop](https://en.wikipedia.org/wiki/Christopher_Bishop). These are just my notes from the book and some selected ideas to form my own narrative. Please refer to the book for full accuracy and details.
