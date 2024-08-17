---
title: "Liouville equation and SDEs"
date: 2024-08-17
---

Recently, I was reading the term "Liouville equation" in the context of SDEs and was not entirely sure what exactly was meant.
For physicists however, the connection of Liouville's equation from statistical mechanics and SDEs might be clear. 

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
\label{empirical}
	\frac{\d}{\d t} \int \phi \d \mu_N(t) &= \frac{\d}{\d t} \frac{1}{N} \sum_i \phi(x_i(t)) =  \frac{1}{N} \sum_i \nabla\phi(x_i(t)) \cdot Y(x_i(t), t)\\
	&= \int \nabla\phi \cdot Y(t, \cdot) \d \mu_N(t).
\end{align} 
$$

Now for each $t$, we can regard the measure $\mu_N(t)$ as a collection of samples representing a non-atomic probability measure.
For that, suppose that for $N \to \infty$ the measure $\mu_N(t)$ converges for all $t$ in weak-star sense to some nice probability measure $\mu(t)$. Assume that $\mu(t)$ has a density $p(t, \cdot)$ with respect to the Lebesgue measure. Applying this to \eqref{empirical} leads to

$$ \begin{align}
\label{transport}
	\frac{\d}{\d t} \int \phi \d \mu(t) &= \frac{\d}{\d t} \int \phi(x) \partial_t p(t, x) \d x = \int \nabla\phi(x) \cdot Y(t, x) p(t,x) \d x\\
	&= - \int \phi(x) \div_x[p(t,x) Y(t,x)] \d x,
\end{align} 
$$

where we have used integration by parts. Since $\phi$ is an arbitrary test function we have

$$ \begin{align} 
\label{liouville}
\partial_t p + \div_x(p Y) = 0,
\end{align}$$

which is a transport equation (also called continuity equation). Note that $p(0, \cdot)$ is given. Equation \eqref{liouville} is called a Liouville equation for the system \eqref{ode}.

The question now is, can we write down a SDE that describes the evolution of $\mu(t)$ with density $p(t, \cdot)$? And vice versa, for a given SDE with fixed initial distribution, can we find a non-autonomous vector field \eqref{ode} describing the evolution of the SDE in the above sense?

tbc.