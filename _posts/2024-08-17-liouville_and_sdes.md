---
title: "Liouville equation and SDEs"
date: 2024-08-17
---

Recently, I was reading the term "Liouville equation" in the context of SDEs and was not entirely sure what exactly was meant.

## Liouville equation

Suppose we have a non-autonomous vector field $Y$ describing an evolution in some phase space:

$$ \begin{align}
	\label{ode}
	x' = Y(x,t)
\end{align} 
$$

We can then follow not only a single initial condition under the flow of \eqref{ode}, but a collection of initial conditions $x_1(0), \dots x_N(0)$, provided their solutions exist for sufficiently long time. Denote the corresponding trajectories by $x_1(t), \dots x_N(t)$ and consider the probability measure $\mu_N$ defined by 

$$ \mu_N(t) = \frac{1}{N} \sum_i \delta_{x_i(t)}.$$

For any test function $\phi$ we then have

$$ \begin{align}
\begin{split}
\label{empirical}
	\frac{\d}{\d t} \int \phi \d \mu_N(t) &= \frac{\d}{\d t} \frac{1}{N} \sum_i \phi(x_i(t)) =  \frac{1}{N} \sum_i \nabla\phi(x_i(t)) \cdot Y(x_i(t), t)\\
	&= \int \nabla\phi \cdot Y(t, \cdot) \d \mu_N(t).
	\end{split}
\end{align} 
$$

Now for each $t$, we can regard the measure $\mu_N(t)$ as a collection of samples representing a non-atomic probability measure.
For that, suppose that for $N \to \infty$ the measure $\mu_N(t)$ converges for all $t$ in weak-star sense to some nice probability measure $\mu(t)$. Assume that $\mu(t)$ has a density $p(t, \cdot)$ with respect to the Lebesgue measure. Applying this to \eqref{empirical} leads to

$$ \begin{align}
\begin{split}
\label{transport}
	\frac{\d}{\d t} \int \phi \d \mu(t) &= \frac{\d}{\d t} \int \phi(x) \partial_t p(t, x) \d x = \int \nabla\phi(x) \cdot Y(t, x) p(t,x) \d x\\
	&= - \int \phi(x) \div_x[p(t,x) Y(t,x)] \d x,
	\end{split}
\end{align} 
$$

where we have used integration by parts. Since $\phi$ is an arbitrary test function we have

$$ \begin{align} 
\label{liouville}
\partial_t p + \div_x(p Y) = 0,
\end{align}$$

which is a transport equation (also called continuity equation). Note that $p(0, \cdot)$ is given. Equation \eqref{liouville} is called a Liouville equation for the system \eqref{ode}.

## SDEs

The question now is: can we write down a SDE that describes the evolution of $\mu(t)$ with density $p(t, \cdot)$? And vice versa, for a given SDE with fixed initial distribution, can we find a non-autonomous vector field \eqref{ode} describing the evolution of the SDE in the above sense?

For this, consider a general SDE
$$
\begin{align}
\label{sde}
\d X_t = a(X_t, t) \d t + b(X_t, t) \d B_t
\end{align}
$$
with the associated Fokker-Planck equation

$$
\begin{align}
\label{fpeq}
\partial_t q = - \sum_i \partial_{x_i}(a q) + \frac{1}{2} \sum_{i,j} \partial_{x_i} \partial_{x_j}( [b b^T]_{ij}\, q ),
\end{align}
$$
where we now denote the density of $X_t$ in \eqref{sde} by $q(t, \cdot)$. Now we can rewrite \eqref{fpeq} to

$$
\begin{align}
\label{fp2}
\partial_t q + \sum_i \partial_{x_i}\left[ \left(  a_i  - \sum_j \left[ \partial_{x_j} ([b b^T]_{ij}) + [b b^T]_{ij} \partial_{x_j}(\log q)\right] \right) q  \right] = 0,
\end{align}
$$
and we see that we can match \eqref{liouville} by choosing the non-autonomous vector field $Y$ to have components

$$Y_i = a_i  - \sum_j [ \partial_{x_j} ([b b^T]_{ij}) + [b b^T]_{ij} \partial_{x_j}(\log q)  ].$$

Note here that the vector field $Y$ does depend on the densities $q(t, x)$ through $\nabla( \log \circ q(t, \cdot))$. 

## And now?

Given a SDE and $\nabla( \log \circ q(t, \cdot))$, where $q$ solves the associated Fokker-Planck equation, a non-autonomous vector field can be constructed, whose flow describes the evolution of an esemble of samples from $q(t, \cdot)$.

tbc.