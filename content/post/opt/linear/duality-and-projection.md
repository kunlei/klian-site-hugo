---
title: Motivating Duality from Projection
date: 2020-07-07
draft: false
tags: ["optimization"]
---

Dual theory is an important topic in linear programming and one that can be motivated from multiple ways.
For example, the dual problem of a primal linear program can be derived using Lagrange multiplier, as elaborated in the book *Introduction to Linear Optimization*.
In this post, I'll show how to come up with the dual formulation with the help of projection.

## Deriving the dual for a primal program in general form

Let's start with a primal problem in general form:

$$
\begin{align}
\text{min. } \quad & \mathbf{c'x} \newline
\text{s.t.} \quad & \mathbf{Ax} \geq \mathbf{b}
\end{align}
$$

Using Fourier-Motzkin elimination to project out all $x$ variables requires a simple reformulation by introducing a new variable $z$ that represents the objective function.

$$
\begin{align}
\text{min.} \quad & z \newline
\text{s.t.} \quad & z - \mathbf{c'x} \geq 0 \newline
 & \mathbf{Ax} \geq \mathbf{b}
\end{align}
$$

Associate a multiplier $u$ and $\mathbf{p}$ with the two constraints respectively and apply projection:

$$
\begin{align}
\text{min.} \quad & z \newline
\text{s.t.} \quad & z \geq \mathbf{p'b}, i = 1, ..., n_1 \newline
 & 0 \geq \mathbf{p'b}, i = 1, ..., n_2
\end{align}
$$

Note that in order for $\mathbf{p}$ to be able to project out the $x$ variables in the primal problem, it must satisfies the following condition:

$$
\begin{align}
\mathbf{p'A} = \mathbf{c} \newline
\mathbf{p} \geq \mathbf{0}
\end{align}
$$

which essentially defines the feasible space for all candidate $\mathbf{p}$.
We also realize that each $\mathbf{p'b}$ provides a lower bound on the primal objective value $z$, for which the optimal value is $\text{max}\\{\mathbf{p'b}\\}$.
In other words, we've successfully constructed the dual problem:

$$
\begin{align}
\text{max.} \quad & \mathbf{p'b} \newline
\text{s.t.} \quad & \mathbf{p'A} = \mathbf{c} \newline
&\mathbf{p} \geq \mathbf{0}
\end{align}
$$

## Deriving the dual for a primal program in standard form

In a similar vein, we can derive the dual for a standard form linear program:

$$
\begin{align}
\text{min. } \quad & \mathbf{c'x} \newline
\text{s.t.} \quad & \mathbf{Ax} = \mathbf{b} \newline
& \mathbf{x} \geq \mathbf{0}
\end{align}
$$

$$
\begin{align}
\text{min.} \quad & z \newline
\text{s.t.} \quad & z - \mathbf{c'x} \geq 0 \newline
 & \mathbf{Ax} = \mathbf{b} \newline
 & \mathbf{x} \geq \mathbf{0}
\end{align}
$$

Projecting out the $x$ variables requires our assigning multiplier $\mathbf{p}$ and $\mathbf{v}$ to the last two constraints that must meet the requirements:

$$
\begin{align}
\mathbf{p'A} + \mathbf{vI} = \mathbf{c} \newline
\mathbf{v} \geq \mathbf{0}
\end{align}
$$

which is equivalent to

$$
\begin{align}
\mathbf{p'A} \leq \mathbf{c} \newline
\end{align}
$$

and then we have the dual problem

$$
\begin{align}
\text{max.} \quad & \mathbf{p'b} \newline
\text{s.t.} \quad & \mathbf{p'A} \leq \mathbf{c} \newline
&\mathbf{p} \ \text{unrestricted}
\end{align}
$$

## Notes

I always find it hard to memorize the correct sign of the dual variable, and this projection provides an intuitive way to assimilate the derivation without much efforts to remember the rules.
For example, if we have a $\mathbf{Ax} \geq \mathbf{b}$ in the primal problem, the dual variable must be nonnegative in order to keep the $\geq$ sign after projection.
As for the $\mathbf{Ax = b}$, the dual variable is not restricted as neither positive nor negative multiplier can alter the $=$ sign of the equation.
