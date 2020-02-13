---
title: Polyhedron
date: 2020-02-13
draft: false
tags: ["opt", "lp"]
---

In this post, we'll review some geometric properties of linear programming.

## Hyperplane

Let $\mathbf{a} = (a_1, a_2, \cdots, a_n)$ be a column vector in $R^n$ and $a_i$ are not all 0, then the set defined by

$$
S = \\{ \mathbf{x} \in \mathcal{R}^n \ | \ \mathbf{a}^T \mathbf{x} = b \\}
$$

is called a *hyperplane*.

Note that the hyperplane defines a $(n-1)$-dimensional space within $R^n$.
In three dimensional space $\mathcal{R}^3$, a hyperplane is a plane.
In two dimensional space $\mathcal{R}^2$, a hyperplane is a line.

## Halfspace

Using the same notations, the following set given by

$$
S = \\{ \mathbf{x} \in \mathcal{R}^n \ | \ \mathbf{a}^T \mathbf{x} \geq b \\}
$$

defines a *halfspace*.

It is easy to see that every constraint in a linear programming formulation effectively defines a halfspace, and the feasible space is decided by the intersection of all the halfspaces.

## Polyhedron

We are now ready to introduce *polyhdron*, a set that can be described as

$$
S = \\{ \mathbf{x} \in \mathcal{R}^n \ | \ \mathbf{A}\mathbf{x} \geq \mathbf{b} \\}
$$

where $\mathbf{A}$ and $\mathbf{b}$ are an $m \times n$ matrix and a $m$-dimensional vector, respectively.

Note that the feasible region of any linear programming problem can be put into a general form of $\mathbf{A}\mathbf{x} \geq \mathbf{b}$, therefore is a polyhedron.
