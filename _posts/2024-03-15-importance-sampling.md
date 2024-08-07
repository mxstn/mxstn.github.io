---
title: "importance sampling"
date: 2024-03-15
---

  \(
\DeclarePairedDelimiter{\norm}{\lVert}{\rVert}
\DeclarePairedDelimiter{\abs}{\lvert}{\rvert}
\renewcommand{\d}{\mathop{}\!\mathrm{d}}
\newcommand{\N}{\mathbb{N}}
\newcommand{\Z}{\mathbb{Z}}
\newcommand{\R}{\mathbb{R}}
\newcommand{\C}{\mathbb{C}}
\newcommand{\iso}{\cong}
\newcommand{\<}{\langle}
\newcommand{\e}{\mathrm{e}}
\renewcommand{\>}{\rangle}
\newcommand{\ext}{\mbox{\Large $\wedge$}}
\DeclareMathOperator{\im}{im}
\DeclareMathOperator{\id}{id}
\DeclareMathOperator{\supp}{supp}
\DeclareMathOperator{\rk}{rk}
\DeclareMathOperator{\tr}{tr}
\DeclareMathOperator{\E}{E}
\DeclareMathOperator{\Var}{Var}
\DeclareMathOperator{\Cov}{Cov}
\DeclareMathOperator{\lin}{span}
\newcommand{\Der}{\mathcal{D}} % tensor derivation D
\newcommand{\Lie}{\mathcal{L}} % lie derivative L
\newcommand{\leval}{\bigg{\rvert}} % large evaluation bar
\newcommand{\eval}{\big{\rvert}} % evaluation bar
\let\div\relax
\DeclareMathOperator{\div}{div}
\DeclareMathOperator{\grad}{grad}
\renewcommand{\H}{\mathcal{H}}
\newcommand{\B}{\mathcal{B}}
\newcommand{\F}{\mathcal{F}}
\newcommand{\D}{\mathcal{D}}
\newcommand{\C}{\mathcal{C}}
\renewcommand{\S}{\mathcal{S}}
\newcommand{\imp}{\Rightarrow}
\newcommand{\pv}{\text{pv}\,}
\newcommand{\eps}{\varepsilon}
\renewcommand{\1}{\mathbbm{1}}
  \)

want to numerically evaluate
\begin{equation}
	\E h(X) = \int h(x) f(x) \d x
\end{equation}
but sampling directly from the density $f$ is hard.

then we can instead sample from some $g$ ($g$ needs to be nonzero where $f$ is nonzero) and use
\begin{equation}
	\label{importance}
	\E h(X) \approx \frac{1}{N} \sum_{i=1}^N h(X_i) \frac{f(X_i)}{g(X_i)} 
\end{equation}

here $g$ shall not have tails lighter than $f$ since $f/g$ will then blow up and some samples can get too large importance.

generally a more stable estimator is to replace $N$ in \eqref{importance} by $\sum_{i=1}^N \frac{f(X_i)}{g(X_i)}$, which can be done since $1/N \sum_{i=1}^N \frac{f(X_i)}{g(X_i)} \approx \int f(x) \d x$ and hence $1/N \sum_{i=1}^N \frac{f(X_i)}{g(X_i)} \to 1$ as $N \to \infty$.

practically good choices for $g$ are such that $\abs{h} f/g$ is roughly constant with finite variance.