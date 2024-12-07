---
title: "Variance estimate error"
date: 2024-12-06
---

Suppose we have a couple of probabilistic samples (say $N$ samples $X_1, \dots, X_N$ ) drawn from a normal distribution. We can estimate the variance of the underlying normal distribution by the sample variance

$$
Z = \frac{1}{N - 1} \sum_{i = 1}^N (X_i - \bar{X})^2,
$$

where $\bar{X} = \frac{1}{N} \sum_i X_i$ is the sample mean. 

A natural question is now: What error to the true variance do we need to expect by using the sample variance $Z$?

Or more precisely, can we give an interval about the true variance, in which the sampled variance will be contained with $x\%$ probability?

At first, this question might seem tricky, but it gets simpler if one knows the


# Distribution of the sample variance

A standard, but very useful fact is that the appropriately scaled sample variance (as a random variable) is distributed according to a chi-squared distribution:

$$
\frac{N-1}{\sigma^2} Z \sim \chi^2_{N-1},
$$

where $\sigma^2$ is the true variance of the normal distribution we sampled from.

But we are interested in the distribution of $Z$ only! Let's get rid of the pre-factors by finding the probability density of $Z$.
Recall that we are dealing with random variables. Denote $Y = \frac{N-1}{\sigma^2} Z$, and by the above result we know the distribution of $Y$.
Then the CDF of $Z$ is given by $p(Z \le z) = p(\frac{\sigma^2}{N-1} Y \le z) = p(Y \le \frac{N-1}{\sigma^2} z)$. One can then see that in fact $Z$ obeys a Gamma distribution with shape $(N-1)/2$ and rate $\frac{N - 1}{2 \sigma^2}$:

$$
Z \sim \text{Gamma}\left( \frac{N-1}{2}, \frac{N - 1}{2 \sigma^2} \right).
$$

Note that we can now infer information about the uncertainty in $Z$.

# But wait!?

Can we say something like: for $N$ drawn samples, the estimated sample variance $Z$ will be within an error of $\pm y\%$ of the true variance with $x\%$ probability?

Roughly, this can be done by employing the quantile function of $Z$, although this does in general not give a symmetric interval about the true variance. Further, the quantiles need to be chosen such that the true variance actually IS contained in the chosen quantile range. But the thing is that we usually do not know the value of $\sigma^2$.

It turns out that the value of $\sigma^2$ does not matter.

Recall that

$$
\begin{equation}
\label{Ze}
Z \sim \text{Gamma}\left( \frac{N-1}{2}, \frac{N - 1}{2 \sigma^2} \right),
\end{equation}
$$

where the gamma distribution $\text{Gamma}(\alpha, \beta)$ has a CDF given by

$$
x \mapsto \frac{1}{\Gamma(\alpha)}\gamma(\alpha, \beta x).
$$

First, note that this immediately implies that $\sigma^2$ is the $q$-quantile of $Z$, where $q = \frac{1}{\Gamma({N-1}/{2})}\gamma({N-1}/{2}, {N-1}/{2})$. This quantile does not depend on $\sigma^2$, only on $N$! So for fixed $N$, we can choose a quantile range $(q_1, q_2)$ with $0 < q_1 < q < q_2 < 1$, which covers $x\%$ of all variance estimates, independent of the value of the true variance.

Let $q_1 = \mathop{cdf}_Z (x_1)$ and $q_2 = \mathop{cdf}_Z(x_2)$ for some value of $\sigma^2$.
Finally, note that the ratios $\frac{x_1}{\sigma^2}$ and $\frac{x_2}{\sigma^2}$ are in fact independent of $\sigma^2$!

To see this, consider two different $\sigma^2$ and $\tilde{\sigma}^2$, and let $f = \tilde{\sigma}^2 / \sigma^2$. Let $\tilde{Z}$ be a random variable as in $\eqref{Ze}$, but with $\sigma^2$ replaced by $\tilde{\sigma}^2$. 
Then $q_{1} = \mathop{cdf}_Z (x_1) = \mathop{cdf}_{\tilde{Z}}(f\cdot x_1)$ and $q_2 = \text{cdf}_{\tilde{Z}}(f \cdot x_2)$, from which the claim follows.

# Conclusion

Having $N$ samples from a normal distribution, we can construct an interval of the form $(y_1\% \text{of true variance}, y_2\% \text{of true variance})$, in which the sample variance estimate lies with $x\%$ probability.

As an example, which is quickly computed in a python notebook, for $N=10$ we have that with $70\%$ probability, our variance estimate is below an error of roughly true variance $\pm 47\%$, as can be seen in the following plot.

<img src="https://mxstn.github.io/assets/img/variance_estimate_error.png" alt="plot" width="100%" oncontextmenu="return false;"/>
