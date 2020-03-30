---
layout: post
title: Infer.NET - Two Children
tags: [Infer.NET, Probability, Bayesian ML]
color: brown
author: wassim
---

Let's explore some basic concepts about probability and [Infer.NET](https://dotnet.github.io/infer/){:target="\_blank"}

## Two boys or two girls

Given two children, what is the probability that the two are boys or the two are girls?

### Probabilities

Since we are not given any extra information about the two children, then each child is equally likely to be a girl or a boy.

So the probability that the first child is a boy can be given by:

$$ p(first\_boy) = 0.5 $$.

And the probability that the second child is a boy can be given by:

$$ p(second\_boy) = 0.5 $$.

From the **_complement rule_** of probability which states that the sum of the probabilities of an event and its complement must equal 1, We get:

$$ p(first\_girl) = 1 - p(first\_boy) = 1 - 0.5 = 0.5 $$.

$$ p(second\_girl) = 1 - p(second\_boy) = 1 - 0.5 = 0.5 $$.

Then the probability that the two children are boys is given by:

$$ p(both\_boys) = p(first\_boy) * p(second\_boy) $$

$$ p(both\_boys) = 0.5 * 0.5 = 0.25 $$

And the probability that the two children are girls is given by:

$$ p(both\_girls) = p(first\_girl) * p(second\_girl) $$

$$ p(both\_girls) = 0.5 * 0.5 = 0.25 $$

### Infer.NET

Let's see how to represent and solve the above problem using [Infer.NET](https://dotnet.github.io/infer/){:target="\_blank"}.

To represent the gender of the child, we can use a binary probability distribution over a boolean value. Where the value of _true_ can represent boy and _false_ can represent _girl_. In _Infer.NET_ we can do that by creating a random variable and specify its prior distribution to be a [Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution){:target="\_blank"} (a type of binary distributions).

```csharp
// a random variable representing the first child is a boy
Variable<bool> first_boy = Variable.Bernoulli(0.5);

// a random variable representing the second child is a boy
Variable<bool> second_boy = Variable.Bernoulli(0.5);
```

Now we can represent the rest of the probabilities using the random variables defined above as follows:

```csharp
// a derived random variable representing the first child is a girl
Variable<bool> first_girl = !first_boy;

// a derived random variable representing the second child is a girl
Variable<bool> second_girl = !second_boy;

// a derived random variable representing both childs are boys
Variable<bool> both_boys = first_boy & second_boy;

// a derived random variable representing both childs are girls
Variable<bool> both_girls = first_girl & second_girl;
```

To calculate the value of the derived random variables _both_boys_ and _both_girls_ (i.e. the distribution of these random variables), we need to perform _inference_. And to perform inference in Infer.NET, we need to use an inference engine.

```csharp
// create an inference engine
InferenceEngine engine = new InferenceEngine();

// perform inference on the random variable both_boys
Console.WriteLine("Probability both childs are are boys: " + engine.Infer(both_boys));

// perform inference on the random variable both_girls
Console.WriteLine("Probability both childs are are girls: " + engine.Infer(both_girls));
```

When run, this code prints out:

```shell
Compiling model...done.
Probability both childs are are boys: Bernoulli(0.25)
Probability both childs are are girls: Bernoulli(0.25)
```
