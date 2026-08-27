---
title: "Diffusion models and stochastic interpolants"
date: 2025-07-05
---

Diffusion models seem to be the todays standard for photo and video generation. The idea is a simple sampling idea: given some training data, which represent samples from a latent probability distribution, we would like to draw new samples according to the underlying law. However, sampling directly from this unknown law is undoable. Instead, the idea is to generate samples from a known distribution, like a normal distribution (or even a Dirac), and suitably transform these samples to somehow obey the latent data distribution. The simple viewpoint adopted in [the paper of Song et al](https://arxiv.org/abs/2011.13456) is that putting data into an Ornstein-Uhlenbeck process converges to a normal distribution. This evolution can be reversed by another SDE, which needs the time-series of the densities of the OU process. A deep neural net is learned to represent this gradient of the logarithm of the time-series of the densities (aka "score function") in order to do this reversion computationally: transform samples of a normal distribution to samples of the training data distribution.

Essentially, it turns out that this viewpoint is in some sense a special case of a more general viewpoint. Namely, designing a suitable deterministic flow or stochastic flow that connects two probability distributions - deterministic in the sense of transport equation (Liouville equation) or stochastic in the sense of a suitable SDE. Such viewpoint is given in [the paper of Albergo et al](https://www.jmlr.org/papers/v26/23-1605.html), which I will follow below.

# stochastic interpolants

Say we have two probability measures $\mu_0$ and $\mu_1$ with corresponding random variables $X_0$ and $X_1$ tow which we have access through samples. Think of $X_0$ as being a normal or a Dirac, and of $X_1$ as being a distribution of images. Define a so-called stochastic interpolant

$$

I_t = \alpha_t X_0 + \beta_t X_1 + \gamma_t Z

$$

for $t \in [0,1]$ with $\alpha, \beta, \gamma$ being $C^1(0,1)$ such that $I_0 = X_0$ and $I_1 = X_1$ and $Z$ a standard normal. In the simplest case, $I_t$ may be a straight-line homotopy between $X_0$ and $X_1$.

Let $\mu_t$ denote the law of $I_t$. This law can roughly be realized by two viewpoints:

# deterministic transport (Liouville) viewpoint

For any test function $\phi$

$$
\frac{\d}{\d t}\leval_t \int \phi \d \mu_t = \frac{\d}{\d t}\leval_t \E[\phi(I_t)] = \E[\frac{\d}{\d t}\leval_t I_t \cdot \nabla \phi(I_t)] = \int \E[\frac{\d}{\d t}\leval_t I_t | I_t = x] \cdot \nabla \phi(x) \d \mu_t(x),
$$

which shows that the distributional derivative of $\mu_t$ is $\E[\frac{\d}{\d t}\leval_t I_t | I_t = x]$.

Hence, the law of $I_t$ and the law of $ X_t = b_t(X_t) \d t $ where $b_t(x) = \E[\frac{\d}{\d t}\leval_t I_t | I_t = x]$ coincide.

Further, it turns out that $b_t(x) = \text{argmin}_{\hat{b}_t} \E[ \abs{\hat{b}_t(I_t)}^2 - 2 \frac{\d}{\d t}\leval_t I_t \cdot \hat{b}_t(I_t) ]$. So $b_t(x)$ can be regressed by a deep neural network. 

Combined with numerical solution of 

$$ 
\begin{equation}
\label{liouville}
X_t = b_t(X_t) \d t 
\end{equation}
$$ 

this leads to a deterministic diffusion model.

# stochastic viewpoint

\eqref{liouville} is the particle version (Liouville equation) equivalent to the associated distributional transport equation $\partial_t \mu_t + \div_x(b_t \mu_t) = 0$.
It also follows that $I_t$ solves certain SDEs. For, starting from the above transport equation, we can derive certain Fokker-Planck equations, which then give the corresponding SDEs.

The ingredients to show this are:

For any test function $\phi$

$$
\E[\nabla \phi(I_t)] = \E[ \phi(I_t) \nabla(\log( \frac{\d \mu_t}{\d x} )) ]
$$

from which it follows that

$$
\nabla(\log(\frac{\d \mu_t}{\d x} )) = - \frac{\E[ Z | I_t = x ]}{\gamma_t} = s_t(x).
$$

Further $\div_x(s_t \mu_t) = \Delta \mu_t$.

By this it follows that $\mu_t$ solves for any continuous $\epsilon_t \ge 0$ the following two Fokker-Planck equations:

First,
$$
\partial_t \mu_t + \div_x(b_t^F \mu_t) = \epsilon_t \Delta \mu_t
$$

with $b_t^F(x) = b_t(x) + \epsilon_t s_t(x)$, which implies that $I_t$ solves

$$
\d X_t = b_t^F(X_t) \d t + \sqrt{2 \epsilon_t} \d B_t
$$
with initial condition $I_0 \sim \mu_0$.

Second,
$$
\partial_t \mu_t + \div_x(b_t^R \mu_t) = - \epsilon_t \Delta \mu_t
$$
with $b_t^R(x) = b_t(x) - \epsilon_t s_t(x)$, which implies that $I_t$ solves

$$
\d X_t = b_t^R(X_t) \d t + \sqrt{2 \epsilon_t} \d B_t
$$
with initial condition $I_1 \sim \mu_1$.

# special case: $X_0 \sim \delta_0$

todo.

optimize $\epsilon_t$ for minimal estimation error

check path KL divergence and minimize

this then leads to 

Föllmer process = Schrödinger bridge between $\delta_0$ and $X_1$

