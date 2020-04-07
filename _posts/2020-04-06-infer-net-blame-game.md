---
layout: post
title: Infer.NET - Blame Game
tags: [Infer.NET, Probability, Bayesian Machine Learning]
color: brown
author: wassim
---

Let's say you went to the office one day and you found that the keyboard you love is missing. Apart from you, there are only two other collegues, Peter and John, who work in the same office. They play a game with you and challenge you to identify the one who took it. What do you do?

## The Suspects

Let's say that you initially, out of some gut feeling, you suspect that it's more likely that John took it. To express this in probability terms, we can represent the identity of the suspect using a random variable whose value can be either John or Peter.

Then the probability that the random variable _suspect_ takes the value _John_ can be written as follows (0.7 is just an arbitrarily chosen value):

$$ p(suspect = John) = 0.7 $$

And since we are certain that it's either _John_ or _Peter_ not any one else and not the two together, then the probability that the random variable _suspect_ takes the value _Peter_ can be written as:

$$ p(suspect = Peter) = 0.3 $$

To represent this in [Infer.NET](https://dotnet.github.io/infer/){:target="\_blank"} we can use a Bernoulli distribution over the variable _suspect_ with a 0.7 probability of true (John) and a 0.3 probability of false (Peter).

```csharp
// a random variable representing the suspect is John (true) / Peter (false)
Variable<bool> suspect = Variable.Bernoulli(0.7);

// create an inference engine
InferenceEngine engine = new InferenceEngine();

// perform inference on the random variable suspect
Console.WriteLine("Probability that John is the suspect: " + engine.Infer(suspect));
Console.WriteLine("Probability that Peter is the suspect: " + engine.Infer(!suspect));
```

When run, this code prints out:

```shell
Compiling model...done.
Probability that John is the suspect: Bernoulli(0.7)
Compiling model...done.
Probability that Peter is the suspect: Bernoulli(0.3)
```

## Inside or Outside

Considering another aspect, which is where the keyboard might have been hidden?! either inside the office room or outside it. Assuming you know Peter is lazy and if he were the suspect then he would most likely hide the keyboard inside the office and won't take the effort to go outside, and say this likelihood is around 90%. On the contrary if it were John who did it then he would most likely take the effort to hide it outside, and say the probability here is 80%.

To capture this new information, we introduce a new random variable _location_ to represent the location that the keyboard might be hidden in. This new variable can take two values _inside_ or _outside_.

Let's capture the relationship between the two variables _location_ and _suspect_ according to the assumptions made above:

$$ p(location=inside | suspect=Peter) = 0.9 $$

Since the only other possibility for _location_ is _outside_ then:

$$ p(location=outside | suspect=Peter) = 0.1 $$

Now for John, the probabilities will be as follows:

$$ p(location=inside | suspect=John) = 0.2 $$

$$ p(location=outside | suspect=John) = 0.8 $$

To represent the new random variable _location_ in Infer.NET, we use another Bernoulli distribution since the variable can take only two possible values, and we extend our model to capture the relationship between the two random variables _location_ and _suspect_.

```csharp
// a random variable representing the location is inside (true) / outside (false)
Variable<bool> location = Variable.New<bool>();

// given the suspect is John
using (Variable.If(suspect))
{
    // p(location=inside | suspect=John) = 0.2
    location.SetTo(Variable.Bernoulli(0.2));
}

// given the suspect is Peter
using (Variable.IfNot(suspect))
{
    // p(location=inside | suspect=Peter) = 0.9
    location.SetTo(Variable.Bernoulli(0.9));
}
```

## The Ringtone

The keyboard has a unique feature, which is when paired with a computer, it can be pinged using the companion software and it will make a ringtone sound! You try it out and ping the keyboard, and ... you can hear the sound!

So now we have evidence that the keyboard is inside the office room not outside it, and we can use this newly learned data into our probabilistic model to refine our probabilities of who hid the keyboard.

We can include the observed evidence in our model, then perform inference to get the posterior probability of the random variable _suspect_.

```csharp
location.ObservedValue = true;

Console.WriteLine("Probability that John is the suspect: " + engine.Infer(suspect));
Console.WriteLine("Probability that Peter is the suspect: " + engine.Infer(!suspect));
```

When run, this code prints out:

```shell
Compiling model...done.
Probability that John is the suspect: Bernoulli(0.3415)
Compiling model...done.
Probability that Peter is the suspect: Bernoulli(0.6585)
```

Unlike our initial assumptions, now we see that the probability that it's Peter is higher than the probability that it's John, and this makes sense since the evidence we gathered points more toward Peter.

## The BackPack

Searching for the source of the sound, you find the keyboard in Peter's backpack. This is a powerful evidence indicating that Peter was the one who took the keyboard, but there is also a very little possibility that the keyboard was taken by John and planted in Peter's backpack to mislead you.

As before, we can capture this new evidence quantitatively using a conditional probability distribution. Let us denote the new information by the random variable _backpack_, which takes the value true if the keyboard is discovered in Peter's backpack, and false otherwise.

Suppose we think there is a 50% chance that Peter would put the keyboard in his own backpack if he were the suspect, but that there is only a 5% chance that John would think to put the keyboard in Peter's backpack if he were the suspect.

$$ p(backpack=true | suspect=Peter) = 0.5 $$

$$ p(backpack=true | suspect=John) = 0.05 $$

We can extend our model to incorporate the new random variable _backpack_ and the relationship it has to the random variable _suspect_:

```csharp
// a random variable representing the keyboard is found in Peter's backpack
Variable<bool> backpack = Variable.New<bool>();

// given the suspect is John
using (Variable.If(suspect))
{
    // p(backpack=true | suspect=John) = 0.05
    backpack.SetTo(Variable.Bernoulli(0.05));
}

// given the suspect is Peter
using (Variable.IfNot(suspect))
{
    // p(backpack=true | suspect=Peter) = 0.5
    backpack.SetTo(Variable.Bernoulli(0.5));
}

backpack.ObservedValue = true;
```

Running the lastest version of our probabilistic model with all the observed evidence gives the following output:

```shell
Compiling model...done.
Probability that John is the suspect: Bernoulli(0.0493)
Compiling model...done.
Probability that Peter is the suspect: Bernoulli(0.9507)
```

Taking account of all of the available evidence, the probability that Peter is the one who took the keyboard is now 95%. And despite all the uncertainties, you have a quantified answer to win the game!
