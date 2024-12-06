---
title: "Variance Estimate Error"
date: 2024-12-06
---

# Estimating the variance of a normal distribution

Suppose we have a couple of probabilistic samples (say $N$) drawn from a normal distribution. We can estimate the variance of the underlying normal distribution by the sample variance

$$
Z = \frac{1}{N - 1} \sum_{i = 1}^N (X_i - \bar{X})^2,
$$

where $\bar{X} = \frac{1}{N} \sum_i X_i$ is the sample mean. 

A natural question is now: What error to the true variance do we need to expect by using the sample variance $Z$?

Or more precisely, can we give an interval about the true variance, in which the sampled variance will be contained with $x\%$ probability?

At first, this question might seem tricky, but it gets extremely simple as one knows the


## Distribution of the sample variance

A standard, but very useful fact is that the appropriately scaled sample variance (as a random variable) is distributed according to a chi-squared distribution:

$$
\frac{N-1}{\sigma^2} Z \sim \chi^2_{N-1},
$$

where $\sigma^2$ is the true variance of the normal distribution we sampled from.

But we are interested in the distribution of $Z$ only! Let's get rid of the pre-factors by finding the probability density of $Z$.
Recall that we are dealing with random variables. Denote $Y = \frac{N-1}{\sigma^2} Z$, and by the above result we know the distribution of $Y$.
Then the CDF of $Z$ is given by $p(Z \le z) = p(\frac{\sigma^2}{N-1} Y \le z) = p(Y \le \frac{N-1}{\sigma^2} z)$ and hence we get that the probability density of $Z$ is given by

$$
z \mapsto \frac{N-1}{\sigma^2} \text{pdf}_Y\left( \frac{N-1}{\sigma^2} z \right),
$$

where $\text{pdf}_Y$ is the known density of $Y \sim \chi^2_{N-1}$. Note that we now know the distribution of $Z$ itself and can infer any information about the uncertainty in $Z$.

## But wait!?

Can we say something like: for $N$ drawn samples, the estimated sample variance $Z$ will be within an error of $\pm y\%$ of the true variance with $x\%$ probability?

Roughly, this can be done by employing the quantile function of $Z$, although this does in general not give a symmetric interval about the true variance. Further, the quantiles need to be chosen such that the true variance actually IS contained in the chosen quantile range.

tbc.
