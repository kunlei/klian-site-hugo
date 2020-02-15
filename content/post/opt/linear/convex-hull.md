---
title: Convexity
date: 2020-02-14
draft: false
tags: ["opt", "lp"]
---

This post describes the concept of convexity.
You've probably heard of convex/concave functions or convex hull, and convexity is indeed an important topic in optimization.

## Convex Set

Let's say we have a set $S$ in $\mathcal{R}^n$ space, it is said to be *convex* if the following condition is met:

$$
\lambda \mathbf{x} + (1-\lambda) \mathbf{y} \in S, \ \forall \mathbf{x}, \mathbf{y} \in S, \lambda \in [0, 1]
$$

This condition basically requires that, for any two points in the set, all the points on the line segment joining the two points must also belong to the set.
If we draw a circle, it is a convex set since any line segments joining two points within the circle also belong to the circle.
It's easy to see that

> The intersection of convex sets is also convex.

The reasoning is straightforward:

> Let $\cap_{i = 1}^k S_i$ indicates the intersection of a group of convex sets $S_i$, and for every points $\mathbf{x}, \mathbf{y} \in \cap\_{i=1}^k S_i$, we know $\lambda \mathbf{x} + (1-\lambda) \mathbf{y} \in S_i, \forall i = 1, \cdots, k$, therefore, $\lambda \mathbf{x} + (1-\lambda) \mathbf{y}$ must also belong to the intersection of $S_i$.

### A halfspace is a convex set

Let $S = \\{ \mathbf{x} \in \mathcal{R}^n \ | \ \mathbf{a}^T \mathbf{x} \geq b \\}$ denote a halfspace, and $\mathbf{x, y} \in S$, and we have

$$
\begin{align}
  \mathbf{a}^T (\lambda \mathbf{x} + (1 - \lambda) \mathbf{y}) &= \lambda \mathbf{a}^T \mathbf{x} + (1 - \lambda) \mathbf{a}^T \mathbf
  {y} \newline
  &\geq \lambda b + (1-\lambda) b \newline
  &\geq b
\end{align}
$$

therefore, $S$ is a convex set.

### A polyhedron is a convex set

Since a polyhedron $S = \\{ \mathbf{x} \in \mathcal{R}^n \ | \ \mathbf{A}\mathbf{x} \geq \mathbf{b} \\}$ is essentially an intersection of multiple halfspaces, and it follows from previous proofs that $S$ is a convex set too.
This is an important conclusion, as every linear programming solution space is a polyhedron and thus convex.

## Convex Combination

A convex set requires the line segment joining any two points lies within the set, and the line segment actually represents all the *convex combinations* of the two points.
The concept of convex combination can be generalized into multiple points: 
let $\mathbf{x}_1, \mathbf{x}_2, \cdots, \mathbf{x}_n \in \mathcal{R}^n $ and $\lambda_1, \lambda_2, \cdots, \lambda_n$ are non-negative scalars, then $\sum\_{i=1}^n \lambda_i x_i$ is called the **convex combination.
## Convex Hull

Given a set of vectors $\mathbf{x}_1, \mathbf{x}_2, \cdots, \mathbf{x}_n \in \mathcal{R}^n $, their *convex hull* is defined as all the convex combinations of th