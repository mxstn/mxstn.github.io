---
title: "Diffusion models and stochastic interpolants"
date: 2025-07-05
---

Diffusion models seem to be the status quo for photo and video generation. I find diffusion models interesting, since they combine dynamics, probability and sampling. Recently, I found a paper giving a nice unified view on diffusion models.

Recall the general idea of diffusion models, which is a simple sampling idea: we would like to draw new samples according to the underlying law of some training data. However, sampling directly from this unknown law is undoable. Instead, the idea is to generate samples from a known distribution, like a normal distribution (or even a Dirac), and suitably transform these samples to get the desired samples. 

The viewpoint adopted in [the paper of Song et al](https://arxiv.org/abs/2011.13456) is that putting data into an Ornstein-Uhlenbeck process converges to a normal distribution. This evolution can be reversed by another SDE, which needs the time-series of the densities of the OU process. A deep neural net is learned to represent this gradient of the logarithm of the time-series of the densities (aka "score function") in order to do this reversion computationally: transform samples of a normal distribution to samples of the training data distribution.

Essentially, it turns out that this viewpoint is in some sense a special case of a more general viewpoint. Namely, designing a suitable deterministic flow or stochastic flow that connects two probability distributions - deterministic in the sense of transport equation (Liouville equation) or stochastic in the sense of a suitable SDE. Such viewpoint is given in [the paper of Albergo et al](https://arxiv.org/abs/2303.08797), which I will follow below.

# stochastic interpolants

Say we have two probability measures $\mu_0$ and $\mu_1$ with corresponding random variables $X_0$ and $X_1$ tow which we have access through samples. Think of $X_0$ as being a normal or a Dirac, and of $X_1$ as being a distribution of images. Define a so-called stochastic interpolant

$$

I_t = \alpha_t X_0 + \beta_t X_1 + \gamma_t Z

$$

for $t \in [0,1]$ with $\alpha, \beta, \gamma$ being $C^1(0,1)$ such that $I_0 = X_0$ and $I_1 = X_1$ and $Z$ a standard normal. In the simplest case, $I_t$ may be a straight-line homotopy between $X_0$ and $X_1$.

Let the measure $\mu_t$ denote the law of $I_t$. This law can roughly be realized by two viewpoints:

# deterministic transport (Liouville) viewpoint

For any test function $\phi$

$$
\frac{\d}{\d t}\leval_t \int \phi \d \mu_t = \frac{\d}{\d t}\leval_t \E[\phi(I_t)] = \E[\frac{\d}{\d t}\leval_t I_t \cdot \nabla \phi(I_t)] = \int \E[\frac{\d}{\d t}\leval_t I_t | I_t = x] \cdot \nabla \phi(x) \d \mu_t(x),
$$

which shows that the distributional derivative of $\mu_t$ is 

$$ \E[\frac{\d}{\d t}\leval_t I_t | I_t = x].$$

Hence, by defining a non-autonomous vector field

$$ b_t(x) = \E[\frac{\d}{\d t}\eval_t I_t | I_t = x],$$

the law of $I_t$ and the law of the solution to $ X_t = b_t(X_t) \d t, X_0 \sim \mu_0 $ coincide.

Further, it turns out that $b_t = \text{argmin}_{\hat{b}_t} \E[ \abs{\hat{b}_t(I_t)}^2 - 2 \frac{\d}{\d t}\eval_t I_t \cdot \hat{b}_t(I_t) ]$, so $b_t(x)$ can be regressed by a deep neural network. 

Combined with numerical solution of 

$$ 
\begin{equation}
\label{liouville}
X_t = b_t(X_t) \d t, \quad X_0 \sim \mu_0
\end{equation}
$$ 

this leads to a deterministic diffusion model.

# stochastic viewpoint

\eqref{liouville} is the particle version (Liouville equation) equivalent to the associated distributional transport equation $\partial_t \mu_t + \div_x(b_t \mu_t) = 0$.
In fact $I_t$ also solves certain SDEs. For, starting from the transport equation, we can derive certain Fokker-Planck equations, which then give the corresponding SDEs.

Define the score function
$$ 
s_t(x) = \nabla(\log(\frac{\d \mu_t}{\d x} )).
$$

The ingredients to derive the Fokker-Planck equations are: for any test function $\phi$, integration by parts implies

$$
\E[\nabla \phi(I_t)] = - \E[ \phi(I_t) \nabla(\log( \frac{\d \mu_t}{\d x} )) ]
$$

and, similarly, for any standard normal random variable $Z$ we have $\E[g(Z) Z] = \E[\nabla_Z g(Z)]$.

By these two facts we can then show that $\E[\phi(I_t) Z] = - \E[\phi(I_t) \gamma_t s_t]$, which, by a density argument and the definition of conditional expectations, implies that

$$
s_t(x) = - \frac{\E[ Z | I_t = x ]}{\gamma_t}.
$$

Finally, it follows that we have $\div_x(s_t \mu_t) = \Delta \mu_t$ in distributional sense.

# the SDEs

By this it follows that $\mu_t$ solves for any continuous function $\epsilon_t \ge 0$ the following two Fokker-Planck equations, and the corresponding SDEs:

First, with $b_t^F(x) = b_t(x) + \epsilon_t s_t(x)$,

$$
\partial_t \mu_t + \div_x(b_t^F \mu_t) = \epsilon_t \Delta \mu_t,
$$

which implies that $I_t$ solves

$$
\d X_t = b_t^F(X_t) \d t + \sqrt{2 \epsilon_t} \d B_t
$$
with initial condition $I_0 \sim \mu_0$.

Second, with $b_t^R(x) = b_t(x) - \epsilon_t s_t(x)$,

$$
\partial_t \mu_t + \div_x(b_t^R \mu_t) = - \epsilon_t \Delta \mu_t,
$$

which implies that $I_t$ solves

$$
\d X_t = b_t^R(X_t) \d t + \sqrt{2 \epsilon_t} \d B_t
$$
with initial condition $I_1 \sim \mu_1$.

# tbc. case: $X_0 \sim \delta_0$

for now open as todo:

optimize $\epsilon_t$ for minimal estimation error

check path KL divergence and minimize

this then leads to 

Föllmer process = Schrödinger bridge between $\delta_0$ and $X_1$

