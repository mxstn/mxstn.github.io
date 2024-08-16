---
title: "Particle filtering"
date: 2024-03-17
---

The first time i came in contact with particle filters was during an internship at KUKA before I obtained my bachelor's degree. At that time, I had no real interest in probability theory, and did not even take a course on it in my bachelor's. Hence, back then, I struggled to understand the theory behind particle filters.

The following are my personal notes on general particle filters, written long after my internship at KUKA.

## Aim

We want to estimate the state of a discrete-time markov process $X_0, X_1, X_2, \dots$, where we do not observe each $X_k$ directly, but instead only observe some $Y_k$. Here $Y_k$ can be thought of as some measurement of $X_k$ corrupted by noise, for example.
This is called a hidden markov model, the process $X_k$ is hidden.

Such scenarios can appear for example from dynamical systems (ODEs or SDEs), which are observed/measured at discrete timesteps:

$$ \begin{align*}
	x' &= Z(x,t) + \text{possibly noise}\\
	x(0) &= x_0.
\end{align*} $$

Another simple special case to keep in mind is a model of the form

$$  \begin{align*}
	X_k &= f(X_{k-1}) + W_{k-1}\\
	Y_k &= g(X_k) + V_k.
\end{align*} $$

## The set-up

To employ a particle filter we need to be able to sample from the transitions $X_{k-1} \to X_k$ represented by $p(x_{k} \rvert x_{k-1})$ and we need to be able to compute the likelihood of $Y_k$ given $X_k$, i.e. $p(y_k \rvert x_k)$.

Suppose we start with some initial distribution $p(x_0) \sim X_0$. Then we want to estimate $ X_1 \rvert Y_0 \sim p(x_1 \rvert y_0)$ and so on. Thus, in the $k$-th step, we want to get from $p(x_k \rvert y_0, \dots y_{k-1})$ to $p(x_{k+1} \rvert y_0, \dots y_k)$. 

This is done in two steps, which constitute the particle filter:

- update $p(x_k \rvert y_0, \dots y_{k-1})$ due to new observation $Y_k$:
$$ \begin{equation}
	p(x_k \rvert y_0, \dots y_{k}) \propto p(y_k \rvert x_k) p(x_k \rvert y_0, \dots y_{k-1})
\end{equation}$$ (posterior is likelihood times prior)

- predict $X_{k+1}$: $$ \begin{equation}
		p(x_{k+1} \rvert y_0, \dots y_{k}) = \int p(x_{k+1}\rvert x_k) p(x_k \rvert y_0, \dots y_{k}) \d x_k
	\end{equation} $$
	
## Approximate by Monte Carlo

The computations in the above two steps can usually not get carried out analytically, since the involved distributions are intractable. Hence, Monte Carlo enters the stage, to represent each distribution by samples. These samples are then called particles.

We will represent the distribution $p(x_{k} \rvert y_0, \dots y_{k-1})$ 
by $N$ weighted particles $\xi^j_k$ ($j = 1, \dots, N$) with weights $w^j_k$. More precisely, for each $k$ and arbitrary $h$, we want to have for $N \to \infty$

$$
\begin{align*}
	\sum_{j=1}^N w^j_k h(\xi^j_k) \to \int h(x_k) p(x_{k} \rvert y_0, \dots y_{k-1}) \d x_k.
\end{align*}
$$

Before the update step $k=0$, we can initialize the weights by $w^{j}_{-1} = 1/N$.

The idea is now to adjust the samples in each of the above two steps accordingly, such that they represent the desired distributions. How exactly these samples are drawn or propagated depends on the underlying model/equations which determine the transition $X_k \to X_{k+1}$ and the likelihood of $Y_k$ given $X_k$. Typically is the following:

- Update step: use [importance (re)sampling](2024-03-15-importance-sampling). Each sample $\xi^j_k$ gets an updated importance weight $w^j_k = \frac{w^j_{k-1} p(y_k\rvert \xi^j)}{\sum_j w^j_{k-1}p(y_k \rvert \xi^j)}$, then we have 
$$ \begin{align*}
		\sum_{j=1}^N w^j_k h(\xi^j_k) \approx \int h(x_k) p(x_k\rvert y_0,\dots y_{k}) \d x_k.
	\end{align*} $$ 
	 In this sense, the collection of weighted samples $(\xi^j_k, w^j_k)$ represents the updated distribution $p(x_k\rvert y_0,\dots y_{k})$.
	 If the effective number of particles is below some prescribed threshold, then we can resample: draw $N$ particles from $\xi^j$ proportional to their weights, and initialize all weights to $1/N$ (which essentially amounts to discarding the weights).
- Prediction step: if the model for the process $X_k$ is noiseless, then just need to propagate the samples through the transition function. Otherwise need to sample from the transition $X_{k} \to X_{k+1}$.

## Background on the update step

The update step is based on [importance sampling](2024-03-15-importance-sampling). The following is a calculation, showing that the update step should be as above. 

First, have $w^j_{-1} = 1/N$ such that

$$
\begin{align*}
	\int h(x_0) p(x_0) \d x_0 \approx 1/N \sum_j h(\xi^j_0)
\end{align*}
$$

then want to approximate 

$$
\begin{align*}
	\int h(x_0) p(x_0 \rvert y_0) \d x_0 \approx 1/N \sum_j h(\xi^{j}_0) p(y_0\rvert x_0)/c = \sum_j w^{j}_{-1} h(\xi^{j}_0) p(y_0\rvert x_0)/c
\end{align*}
$$

where $c$ needs to be found. for example if $h=1$ then the integral equals 1, so $c$ should be $\sum_j p(y_0\rvert \xi^j_0) /N = \sum_j w^{j}_{-1} p(y_0\rvert \xi^j_0)$. so indeed

$$
\begin{align*}
	w^j_0 = \frac{w^{j}_{-1} p(y_0\rvert x_0)}{\sum_j w^{j}_{-1} p(y_0\rvert \xi^j_0)}.
\end{align*}
$$

at step $k$ the argument continues along these lines: now have

$$
\begin{align}
	\label{insert_f}
	\int h(x_k) p(x_k \rvert y_0, \dots y_{k-1}) \d x_k \approx \sum_j w^{j}_{k-1} h(\xi^{j}_k)
\end{align}
$$

now since 

$$
\begin{align}
	\label{cpdef}
	p(x_k \rvert y_0, \dots y_{k}) = \frac{p(y_k \rvert x_k) p(x_k \rvert y_0, \dots y_{k-1})}{\int p(y_k \rvert x_k) p(x_k \rvert y_0, \dots y_{k-1}) \d x_k}
\end{align}
$$

want to add a factor in the right hand side sum of $\eqref{insert_f}$ to get

$$
\begin{align*}
	\int h(x_k) p(x_k \rvert y_0, \dots y_{k}) \d x_k \approx \sum_j w^{j}_{k-1} \frac{p(y_k \rvert \xi^{j}_k)}{c} h(\xi^{j}_k) = \sum_j w^{j}_{k} h(\xi^{j}_k)
\end{align*}
$$

thus $c$ comes from the integral in the denominator of \eqref{cpdef}, which is approximated itself by $\sum_j w^{j}_{k-1} p(y_k \rvert \xi^j_k)$. hence

$$
\begin{align*}
	w^j_k = \frac{w^j_{k-1} p(y_k\rvert \xi^j)}{\sum_j w^j_{k-1}p(y_k \rvert \xi^j)}.
\end{align*}
$$