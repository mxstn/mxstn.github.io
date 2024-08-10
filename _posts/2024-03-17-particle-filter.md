---
title: "my view on particle filtering"
date: 2024-03-17
---

the first time i came in contact with particle filters was during an internship at KUKA before i obtained my bachelor's degree. at that time, i was still missing the mathematical maturity to understand the theory behind particle filters. also, at that time, i had no interest at all in probability theory, and did not even take a course on it in my bachelor's!

the following are my personal notes on general particle filters, written 8 years after my internship at KUKA ;)

## aim

want to estimate the state of a discrete-time markov process $X_0, X_1, X_2, \dots$, where we do not observe each $X_k$ directly, but instead only observe $Y_k$. this is a hidden markov model, the process $X_k$ is hidden.

such scenarios appear for example from dynamical systems, which are observed/measured at discrete timesteps:

$$ \begin{align*}
	x' &= Z(x,t) + \text{possibly noise}\\
	x(0) &= x_0
\end{align*} $$

a simple case to keep in mind is a model of the form

$$  \begin{align*}
	X_k &= f(X_{k-1}) + W_{k-1}\\
	Y_k &= g(X_k) + V_k
\end{align*} $$

## set up

to employ a particle filter we need to be able to sample from the transitions $X_{k-1} \to X_k$ represented by $p(x_{k} \rvert x_{k-1})$ and need to compute the likelihood of $Y_k$ given $X_k$, i.e. $p(y_k \rvert x_k)$.

suppose we start with some initial distribution $p(x_0) \sim X_0$. we then want to estimate $ X_1 \rvert Y_0 \sim p(x_1 \rvert y_0)$ and so on. thus we want to get from $p(x_k \rvert y_0, \dots y_{k-1})$ to $p(x_{k+1} \rvert y_0, \dots y_k)$. 

this is done in two steps:

- update $p(x_k \rvert y_0, \dots y_{k-1})$ due to new observation $Y_k$:
$$ \begin{equation}
	p(x_k \rvert y_0, \dots y_{k}) \propto p(y_k \rvert x_k) p(x_k \rvert y_0, \dots y_{k-1})
\end{equation}$$ (posterior is likelihood times prior)

- predict $X_{k+1}$: $$ \begin{equation}
		p(x_{k+1} \rvert y_0, \dots y_{k}) = \int p(x_{k+1}\rvert x_k) p(x_k \rvert y_0, \dots y_{k}) \d x_k
	\end{equation} $$
	
## approximate by monte carlo

these computations can usually not get carried out directly. monte carlo enters the stage, to represent each distribution by samples, called particles.

take $N$ weighted particles $\xi^j_k$ 
($j = 1, \dots, N$) with weights $w^j_k$ to act as samples of $p(x_{k} \rvert y_0, \dots y_{k-1})$ for each $k$ in the sense of

$$
\begin{align*}
	\sum_{j=1}^N w^j_k h(\xi^j_k) \approx \int h(x_k) p(x_{k} \rvert y_0, \dots y_{k-1}) \d x_k
\end{align*}
$$

the weights before update step $k=0$ are initialized by $w^{j}_{-1} = 1/N$.

how these samples are drawn or propagated depends on the underlying model/equations which determine the transition $X_k \to X_{k+1}$ and the likelihood of $Y_k$ given $X_k$. typically is the following:

- update step: use importance (re)sampling. each $\xi^j_k$ gets an updated importance weight $w^j_k = \frac{w^j_{k-1} p(y_k\rvert \xi^j)}{\sum_j w^j_{k-1}p(y_k \rvert \xi^j)}$, then have 
$$ \begin{align*}
		\sum_{j=1}^N w^j_k h(\xi^j_k) \approx \int h(x_k) p(x_k\rvert y_0,\dots y_{k}) \d x_k
	\end{align*} $$ in this sense $(\xi^j_k, w^j_k)$ represent the updated distribution $p(x_k\rvert y_0,\dots y_{k})$.
	 if the effective number of particles is below some prescribed threshold, then we can resample: draw $N$ particles from $\xi^j$ proportional to their weights, and initialize all weights to $1/N$ (which amounts to discarding the weights).
- prediction step: if the model for the process $X$ is noiseless, then just need to propagate the samples through the transition function. otherwise need to sample from the transition $X_{k-1} \to X_k$.

## background on the two steps

calculation why the update step should be this way: first have $w^j_{-1} = 1/N$ such that

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