---
title: "Takens' normal form theorem"
date: 2024-09-21
---

Recently I revisited [Floris Takens' massive paper on singularities](https://eudml.org/doc/103931). In this paper he formulates a normal form theorem for vector fields (similar to the Poincare normal form) and essentially gives a coordinate-free proof. The version I cite below is the slightly generalized version as stated in the appendix of the paper. I also adjusted the proof slightly to use pullbacks instead of pushforwards, as pullbacks are more convenient when working with Lie derivatives.

## Set-up for the theorem

Let $X$ be a vector field on $\R^d$ of smoothness class $C^k$, vanishing at the origin: $X(0) = 0$. Let $s$ be the smallest integer such that the $s$-jet of $X$ at 0 is not vanishing. Denote by $X_s$ the vector field given by the $s$-jet of $X$ at 0, which has homogeneous polynomials of order $s$ as its components.

Let $H^h$ denote the space of vector fields, whose components are homogeneous polynomials of order $h$. Then consider for some vector field $Y \in H^h$ the Lie bracket $[ X_s , Y]$ and note that

$$  
[ X_s , Y] \in H^{s+h-1}.
$$

Finally, denote by $B^h$ the image of the map

$$ [ X_s, \cdot] \colon H^{h-s+1} \to H^h$$

for $h>s$, and choose a complement $G^h$ such that 

$$H^h = B^h \oplus G^h.$$

## The assertion

For $l \le k$ there exists a local diffeomorphism $\psi$ with $\psi(0) = 0$ such that

$$
\psi_\ast(X) = X_s + g_{s+1} + \dots + g_l + R_l,
$$

where each $g_m \in G^m$ and $R_l$ has vanishing $l$-jet at 0.

## The proof

For $l = s$ the assertion holds, so we can proceed by induction. Assume we have given

$$
X = X_s + g_{s+1} + \dots + g_{l-1} + R_{l-1}.
$$

Then we can write $R_{l-1} = g_l + b_l + R_l$ for $g_l \in G^l, b_l \in B^l$, such that

$$
X = X_s + g_{s+1} + \dots + g_{l} + b_l + R_{l}.
$$

The goal is now to transform the summand $b_l$ away.

By the above construction there is $Y \in H^{l-s+1}$ such that $b_l = [X_s, Y]$. Now define

$$
X_t = \left( \varphi^t_Y \right)^\ast (X),
$$

where $\varphi^t_Y$ is the flow of $Y$. The claim is now that $X_{t=1}$ takes the desired form.

### proof: $X_{t=1}$ does  the job

We want to employ a Taylor expansion for $X_t$ valid for $t \in [0,1]$. First, note that the definition of $X_t$ is only missing the time derivative to be in the form of a Lie derivative.

Now compute

$$
\begin{align*}
\frac{\d}{\d t}\leval_{t=\tau} X_t &= \frac{\d}{\d t}\leval_{t=0} (\varphi^\tau_Y)^\ast X_t\\
&= \L_Y (\varphi^\tau_Y)^\ast X\\
&= [Y, X_\tau],
\end{align*}
$$

and observe that we can continue iteratively

$$
\begin{align*}
\frac{\d}{\d t}\leval_{t=\tau} [Y, X_t] &= \frac{\d}{\d t}\leval_{t=0} (\varphi^\tau_Y)^\ast [Y, X_t]\\
&= \L_Y (\varphi^\tau_Y)^\ast [Y, X]\\
&= [Y, [Y, X_\tau]].
\end{align*}
$$

Hence the Taylor expansion of $X_t$ in $t$ is of the form

$$
X_t = X + t [Y, X_t] + \frac{t^2}{2} [Y, [Y, X_t]] + \dots
$$

and equality holds for $t \in [0,1]$ by adding an appropriate (higher-order) remainder term.

Now

$$
\begin{align*}
[Y, X_t] &= (\varphi^t_Y)^\ast [Y, X]\\
&= (\varphi^t_Y)^\ast \left( [Y,X_s] + [Y, g_{s+1}] + \dots + [Y, R_l] \right),
\end{align*}
$$

where $[Y,X_s] = - b_l$ and $[Y, g_{s+1}] + \dots + [Y, R_l]$ has a vanishing $l$-jet. 

Now one can again use a Taylor expansion of $[Y, X_t]$ in $t$

$$
[Y, X_t] = [Y, X] + t [Y, [Y, X_t]] + \dots
$$ 

to finally see that

$$
[Y, X_t] = -b_l + R_{l,t},
$$

where $R_{l,t}$ has vanishing $l$-jet.

Hence

$$
X_t = X - t b_l + \text{h. o. t.},
$$

so $X_1$ indeed takes the desired form $X_1 = X_s + g_{s+1} + \dots + g_{l} + R_{l}$.

## Conclusion

For $l \le k < \infty$ the above procedure gives a finite series of local diffeomorphisms, i.e. local coordinate changes, such that their concatenation puts the vector field in the desired form.

However, the theorem also holds for $l = k = \infty$, in which case we get an infinite series of local diffeomorphisms. In this case Borel's lemma can be used.