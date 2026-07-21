---
title: "Carré du champ operator and Riemannian metric"
date: 2026-07-15
---

Very recently, at ICML 2026, I came across the carré du champ operator in [this ML paper](https://openreview.net/forum?id=KVzXnWPLgX) and I got hooked immediately. 
Let me try to explain :)


# World of smooth manifolds

Consider the Laplace-Beltrami operator $\Delta$ on a Riemannian manifold, which on functions $f$ is given by $\Delta f = \div \nabla f$ (there exist different sign and scaling conventions). Then one can check that the following identity holds:

$$
\begin{equation}

\label{laplace_product}

\Delta(f h) = f \Delta g + g \Delta f + 2 g(\nabla f, \nabla g),

\end{equation}
$$

where $g$ is the Riemannian metric. This means that the metric $g$ can be given in terms of Laplace-Beltrami $\Delta$.


# World of rough stochastic processes

Now say we are in the rough world of stochastic processes. Say we have a Markov process $X_t$. The law of $X_t$ is determined by testing with measurable functions $f$ against the measure determined by $X_t$. Now we can mimic a derivative, similar to a derivative of a flow which gives a vector field. Since we are in the probabilistic, rough setting, we can consider a kind of conditional expectation derivative like

$$
\lim_{\tau \downarrow 0} \frac{1}{\tau} \E [f(X_{t+\tau}) - f(X_t) | X_t = x]
$$

Then if we have time-homogeneity, meaning that it does not matter at which time exactly we start the process, only the evolved time matters, we obtain the operator

$$
A\colon f \mapsto \left( x \mapsto \lim_{\tau \downarrow 0} \frac{1}{\tau} \E [f(X_{\tau}) - f(x)] \right)
$$

$A$ is called the infinitesimal generator. Note the analogy of flows and vector fields, where vector fields are sometimes also called infinitesimal generators. Now, the so called carré du champ operator can be defined by (there exist different sign and scaling conventions)

$$
\Gamma(f,h) = 1/2 (f Ah + hAf - A(f h))
$$

Note that if $A$ would obey the Leibniz rule, i.e. would be a derivation, then $\Gamma = 0$.

# The main idea

Now in view of $\eqref{laplace_product}$, the main idea is to use the carré du champ operator $\Gamma$ to define a Riemannian metric. 

Note that in this stochastic process setting, we have neither a notion of tangent space at a certain point nor a derivative of $f$ and $h$ available. However, $\Gamma(f, h)$ essentially tells us reasonable values of a Riemannian metric determined by $A$ on a probability space. 

Essentially, we formally have
$$
\Gamma(f,h) = g(\nabla f, \nabla h) = g((\d f)^\sharp, (\d h)^\sharp).
$$

The question is: how to define the expressions on the right hand sides in a suitable way? 

This is done in the impressive paper [diffusion geometry](https://arxiv.org/abs/2405.10858). The strategy seems to go as follows:

* abstractly define objects like $\d f$, $\d h$ called 1-forms (caution: these things do at first not have any linear action)
* define a metric on these abstract 1-forms
* define vector fields as derivations, which correspond to 1-forms through the metric
* define the action of 1-forms through the metric

... and so forth. By this, it seems possible to introduce Riemannian geometry on probability spaces! In particular, samples from euclidean probability spaces, e.g. point clouds, can be used to compute the Carré du champ operator from the heat diffusion operator on the ambient space, by results from [this paper](https://www.sciencedirect.com/science/article/pii/S1063520306000546). This opens up a world of Riemannian geometry for point clouds based on Markov diffusion operators.
