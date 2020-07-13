---
title: Benders Decomposition
date: 2020-07-12
draft: false
tags: ["optimization", "decomposition"]
---

Benders decomposition is a potent, albeit advanced, technique to attack optimization problems with certain special structures.
Specifically, it's applicable when a model possesses the following structure:

$$
\begin{align}
\mbox{min.} \quad & \mathbf{c'x} + \mathbf{f'y} \newline
\mbox{s.t.} \quad & \mathbf{Ax} + \mathbf{By} \geq \mathbf{b} \newline
& \mathbf{x} \geq 0 \newline
& \mathbf{y} \in \mathbf{Y}
\end{align}
$$

where $\mathbf{x}$ and $\mathbf{y}$ are decision variables in $\mathbf{R}^{n_1}$ and $\mathbf{R}^{n_2}$, respectively.
$\mathbf{c}$ and $\mathbf{f}$ are column vectors of size $n_1$ and $n_2$, and matrices $\mathbf{A}$ and $\mathbf{B}$ are of size $m \times n_1$ and $m\times n_2$, respectively.
$\mathbf{b}$ is a column vector of size $m$.
Note that $\mathbf{x}$ are nonnegative continuous variables, whereas $\mathbf{y}$ might be discrete variables.

## Motivating Benders decomposition through projection

The specialty of problems in this structure is that, with $\mathbf{y}$ fixed, the remaining problem with only the $\mathbf{x}$ variables is relatively easy to solve.
Introduce a new variable $z$ to denote the objective and rearrange the constraints, we have 

$$
\begin{align}
\mbox{min.} \quad & z \newline
\mbox{s.t.} \quad & z - \mathbf{c'x} \geq \mathbf{f'y} \newline
& \mathbf{Ax} \geq \mathbf{b} - \mathbf{By} \newline
& \mathbf{x} \geq 0 \newline
& \mathbf{y} \in \mathbf{Y}
\end{align}
$$

Associating a nonnegative multiplier $u$ to constraints $z - \mathbf{c'x} \geq \mathbf{f'y}$, vector $\mathbf{v}$ to constraints $\mathbf{Ax} \geq \mathbf{b} - \mathbf{By}$, and vecotr $\mathbf{w}$ to $\mathbf{x} \geq 0$, we can project out the $\mathbf{x}$ variables using Fourier-Motzkin elimination that results in 

$$
\begin{align}
\mbox{min.} \quad & z \newline
\mbox{s.t.} \quad & u^iz \geq u^i\mathbf{f'y} + \mathbf{v^i(b - By)}, i = 1,\cdots, K  \newline
& \mathbf{y} \in \mathbf{Y}
\end{align}
$$

where $K$ is the number of possible multipliers that meet the following criteria:

$$
\begin{align}
\mathbf{v'A} + \mathbf{w'} = u\mathbf{c'}
\end{align}
$$

Given that $u \geq 0$, the first constraints can be subsumed into two categories based on whether $u = 0$ or $u \gt 0$:

$$
\begin{align}
\mbox{min.} \quad & z \newline
\mbox{s.t.} \quad & z \geq \mathbf{f'y} + \mathbf{v^i(b - By)}, i = 1,\cdots, K_1  \newline
& 0 \geq \mathbf{v^i(b - By)}, i = K_1 + 1,\cdots, K  \newline
& \mathbf{y} \in \mathbf{Y}
\end{align}
$$

Note that the above model, hereafter we refer it as the main problem, is essentially the original model projected into the space $\mathbf{y} \in \mathbf{R}^{n_2}$, and $\mathbf{K}$ is the total number of constraints that define the feasible region of the projected model.
Unfortunately, $K$ is usually prohibitively large for practical size problems.

## Delayed constraint generation

