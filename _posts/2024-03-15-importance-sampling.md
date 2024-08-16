---
title: "importance sampling"
date: 2024-03-15
---

A basic probabilistic way to evaluate expectations (integrals) numerically is the usual Monte Carlo method, which is essentially just the law of large numbers.

Importance sampling can be thought of as a extension to this. Say we want to numerically evaluate
\begin{equation}
	\E h(X) = \int h(x) f(x) \d x,
\end{equation}
but sampling directly from the density $f$ is hard.

Then we can instead sample from some $g$ ($g$ needs to be nonzero where $f$ is nonzero), and use
\begin{equation}
	\label{importance}
	\E h(X) \approx \frac{1}{N} \sum_{i=1}^N h(X_i) \frac{f(X_i)}{g(X_i)}.
\end{equation}

Here $g$ shall not have tails lighter than $f$, since $f/g$ will then blow up and some samples can get too large importance.

Generally, a more stable estimator is to replace $N$ in \eqref{importance} by $\sum_{i=1}^N \frac{f(X_i)}{g(X_i)}$, which can be done since $1/N \sum_{i=1}^N \frac{f(X_i)}{g(X_i)} \approx \int f(x) \d x$ and hence $1/N \sum_{i=1}^N \frac{f(X_i)}{g(X_i)} \to 1$ as $N \to \infty$.

Practically, good choices for $g$ are such that $\abs{h} f/g$ is roughly constant with finite variance.