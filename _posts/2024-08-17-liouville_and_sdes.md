---
title: "Liouville equation and SDEs"
date: 2024-08-17
---

Recently, I was reading the term "Liouville equation" in the context of SDEs and was not entirely sure what exactly was meant. Turns out a Liouville equation can be seen as a transport equation, describing the evolution of a probability distribution "of particles" under given deterministic dynamics. A nice way to derive this equation is to use a Monte Carlo approach: consider an esemble of samples and track their evolution.

## Liouville equation from samples

Suppose we have a sufficiently regular, non-autonomous vector field $Y$:

$$ \begin{align}
	\label{ode}
	\frac{\d x}{\d t} = Y(x,t)
\end{align} 
$$

As is, this is a "particle-wise" description. We can then follow not only a single initial condition under the flow of \eqref{ode}, but a collection of initial conditions $x_1(0), \dots x_N(0)$, provided their solutions exist for sufficiently long time. These points will serve as samples, approximating some initial distribution of particles at time $t=0$.
Denote the corresponding trajectories by $x_1(t), \dots x_N(t)$ and consider the time-dependent probability measure $\mu_{N,t}$ defined by 

$$
\begin{align}
\label{mc}
 \mu_{N,t} = \frac{1}{N} \sum_i \delta_{x_i(t)}.
 \end{align}$$

For any spatial test function $\phi$ we then have

$$ \begin{align}
\label{empirical}
	\frac{\d}{\d t}\leval_t \int \phi \d \mu_{N,t} &= \frac{\d}{\d t}\leval_t \frac{1}{N} \sum_i \phi(x_i(t)) =  \frac{1}{N} \sum_i \nabla\phi(x_i(t)) \cdot Y(x_i(t), t)\\
	&= \int \nabla\phi \cdot Y(t, \cdot) \d \mu_{N,t}. \nonumber
\end{align} 
$$

Now suppose we start with some general probability distribution $\mu_0$ of particles at time $t = 0$ and we are interested in its evolution $\mu_t$ under the dynamics \eqref{ode}.
Consider Monte Carlo approximations to $\mu_t$ of the form \eqref{mc}.
In this case we have that for $N \to \infty$ the measures $\mu_{N,t}$ converge for each $t$ in weak-star sense to $\mu_t$.

Then, if the measures $\mu_t$ have sufficiently smooth densities $p(t, \cdot)$, using \eqref{empirical} we get

$$ \begin{align}
\begin{split}
\label{transport}
	\frac{\d}{\d t}\leval_t \int \phi \d \mu_t &= \int \phi(x) \partial_t p(t, x) \d x = \int \nabla\phi(x) \cdot Y(t, x) p(t,x) \d x\\
	&= - \int \phi(x) \div_x[p Y](t,x) \d x,
	\end{split}
\end{align} 
$$

where we have used integration by parts. Since $\phi$ is an arbitrary test function and $t$ was arbitrary, we get

$$ \begin{align} 
\label{liouville}
\partial_t p + \div_x(p Y) = 0,
\end{align}$$

which is a transport equation (also called continuity equation). Note that $p(0, \cdot)$ is given. Equation \eqref{liouville} can be called a Liouville equation for the system \eqref{ode}. Note that for general probability measures equation \eqref{liouville} must be read in distributional sense.

System \eqref{ode} can also be seen as Liouville equation itself, since one may express $\mu_t$ via the pushforward $\mu_t = (\varphi^{t,0})_\ast \mu_0$, at least locally, where the flow $\varphi^{t,0}$ of $Y$ exists, [check below](#appendix).

## Connection to SDEs?

Stochastic differential equations (SDEs) can also describe the evolution of probability distributions. How do they relate to Liouville equations?
Given a Liouville equation, can we write down a SDE that describes the evolution of $\mu_t$? 
And vice versa, for a given SDE with fixed initial distribution, can we find a non-autonomous vector field \eqref{ode} also describing the evolution of the distribution in the above sense?
A way to answer this is to look at the Fokker-Planck equation.

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
\partial_t q = - \sum_i \partial_{x_i}(a_i q) + \frac{1}{2} \sum_{i,j} \partial_{x_i} \partial_{x_j}( [b b^T]_{ij}\, q ),
\end{align}
$$
where we now denote the density of $X_t$ in \eqref{sde} by $q(t, \cdot)$. Now we can rewrite \eqref{fpeq} to

$$
\begin{align}
\label{fp2}
\partial_t q + \sum_i \partial_{x_i}\left[ \left(  a_i  - \frac{1}{2} \sum_j \left[ \partial_{x_j} ([b b^T]_{ij}) + [b b^T]_{ij} \partial_{x_j}(\log q)\right] \right) q  \right] = 0,
\end{align}
$$
if $q$ does not vanish, where the key ingredient is $ \nabla_x \log q(t,x) = \frac{\nabla_x q(t, x)}{q(t, x)}$.

Now we see that we can match \eqref{liouville} by choosing the non-autonomous vector field $Y$ to have components

$$Y_i = a_i  - \frac{1}{2} \sum_j [ \partial_{x_j} ([b b^T]_{ij}) + [b b^T]_{ij} \partial_{x_j}(\log q)  ].$$

Note here that the vector field $Y$ does depend on the densities $q(t, x)$ through $\nabla( \log \circ q(t, \cdot))$. 

Hence, given a SDE and $\nabla( \log \circ q(t, \cdot))$, where $q$ solves the associated Fokker-Planck equation, a non-autonomous vector field can be constructed, such that the associated Liouville equation describes the evolution of $q(t, \cdot)$.

## Appendix

Let $\varphi^{t,0}(x)$ denote the flow of $Y$ (meaning $ \frac{\d}{\d t}\eval_t \varphi^{t,0}(x) = Y(t, \varphi^{t, 0}(x)) $)
and let $\mu_t = (\varphi^{t,0})_\ast \mu_0$. 

The following lines are to show that $\mu_t$ solves \eqref{liouville} in distributional sense (at least locally where the flow exists).

Take some test function $\psi$ in space-time. Then we have

$$
\begin{align*}
\frac{\d}{\d t}\eval_t \psi(t, \varphi^{t,0}(x)) = \partial_t \psi(t,\varphi^{t,0}(x)) + \nabla_x \psi(t, \varphi^{t,0}(x)) \cdot Y(t, \varphi^{t, 0}(x)).
\end{align*}
$$

Let $ \langle \xi , \psi \rangle $ denote the value obtained from testing a distribution $\xi$ against $\psi$ (here, distribution is meant in the sense of distribution theory). Then, by slight abuse of notation

$$
\begin{align*}
\< \partial_t \mu_t + \div_x(\mu_t Y), \psi \> &= - \< \mu_t, \partial_t \psi + \nabla_x \psi \cdot Y \> \\
&= - \< (\varphi^{t,0})_\ast \mu_0, \partial_t \psi + \nabla_x \psi \cdot Y \> \\
&= - \< \mu_0, (\partial_t \psi + \nabla_x \psi \cdot Y) \circ \varphi^{t,0} \> \\
&= - \int \int \frac{\d}{\d t}\leval_t \psi(t, \varphi^{t,0}(x)) \d \mu_0(x) \d t\\
&= - \int \frac{\d}{\d t}\leval_t \int \psi(t, \varphi^{t,0}(x)) \d \mu_0(x) \d t\\
&= 0,
\end{align*}
$$

where we have used dominated convergence and the fact that $\psi$ has compact support. Hence $\mu_t$ solves \eqref{liouville} in distributional sense, since $\psi$ was arbitrary.

Note that the above lines are abuse of notation, since $\mu_t$ seen as distribution is only acting on test functions in spatial variables. The technically correct way is to write $\langle 1 \otimes \mu_t, \psi \rangle$ instead of $\langle \mu_t, \psi \rangle$, since $\psi(t,x)$ is a test function on space-time. Technically precise, this would also mean that $1 \otimes \mu_t$ solves \eqref{liouville} in distributional sense.



