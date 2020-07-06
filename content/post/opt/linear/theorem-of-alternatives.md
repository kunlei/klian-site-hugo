---
title: Deriving Theorems of the Alternatives using Projection
date: 2020-07-06
draft: false
tags: ["optimization"]
---

The theorem of the alternatives is an important topic in linear programming, but I found it utterly confusing, not about how it works, and more about how the alternatives are derived in the first place.
The textbook used in my linear programing class simply states the two alternatives, the Farkas' Lemma, and proves its correctness, which left me bewildered about its derivation.
It is not until I learned about the theory of projection that I started to understand the various theorems of alternatives.

## Get to our first theorem of alternatives using projection

To explain this, suppose we have a system of inequalities of the following form

$$
\begin{align}
\mathbf{Ax} &\geq \mathbf{b}
\end{align}
$$

where $\mathbf{A}$ is a $m \times n$ matrix, $\mathbf{x}$ is column vector in $\mathbf{R}^n$ and $\mathbf{b}$ is a constant vector in $\mathbf{R}^m$.
Eliminating the variables $\mathbf{x}$ from the left hand side is equivalent to find a multiplier vector $\mathbf{p}$, a column vector of size $m$, such that $\mathbf{p'A} = \mathbf{0}$ and $\mathbf{p'b} \leq 0$, which is essentially the process of projection that drives the original system of inequalities into the form below

$$
\begin{align}
0 \geq \mathbf{p'b}
\end{align}
$$

Note that if we can find a vector $\mathbf{p}$ such that $\mathbf{p'A = 0}$ and $\mathbf{p'b} > 0$, then the original system of inequalities must have no solution.
Now we have our first theorem of the alternatives:

**Theorem 1.** Either the system $\mathbf{Ax} \geq \mathbf{b}$, or the system $\mathbf{p} \geq 0, \mathbf{p'A = 0}, \mathbf{p'b} > 0 $ has a solution, but not both.

## Derive variations

The theorem of the alternatives has multiple variations, but all of them can be derived through similar reasoning logic.
Now let's look at the constraint set of a linear program in standard form $\mathbf{Ax = b}$ and $\mathbf{x} \geq \mathbf{0}$.
First we rewrite it into the following format

$$
\begin{align}
\mathbf{Ax} &\geq \mathbf{b} \newline
\mathbf{-Ax} &\geq \mathbf{-b} \newline
\mathbf{x} &\geq \mathbf{0}
\end{align}
$$

Associate the multiplier $\mathbf{u, v, w} \geq 0$ and apply the projection procedure, we have

$$
\begin{align}
\mathbf{u'A} - \mathbf{v'A} + \mathbf{w'I} &= 0 \newline
\mathbf{u'b} - \mathbf{v'b} &> 0
\end{align}
$$

Let $\mathbf{p = u - v}$, then we have $\mathbf{p'A} + \mathbf{w'I} = 0$ and $\mathbf{p'b} \geq 0$.
Now we have the second theorem of the alternatives:

**Theorem 2.** Either the system $\mathbf{Ax = b}, \mathbf{x} \geq \mathbf{0}$ has a solution, or the system $\mathbf{p'A} \leq \mathbf{0}, \mathbf{p'b} \geq 0$ has a solution, but not both.
