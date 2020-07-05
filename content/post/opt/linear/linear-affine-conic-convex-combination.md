---
title: Linear, Affine, Conic and Convex Combinations
date: 2020-06-29
draft: false
tags: ["polyhedron"]
---

The **linear combination** of a set of points $x_1, x_2, \cdots, x_k$ in $\mathcal{R}^n$ is defined as

$$
\lambda_1 x_1 + \lambda_2 x_2 + \cdots + \lambda_k x_k
$$

The **affine combination** of a set of points $x_1, x_2, \cdots, x_k$ in $\mathcal{R}^n$ is defined as

$$
\lambda_1 x_1 + \lambda_2 x_2 + \cdots + \lambda_k x_k, \sum\_{i = 1}^k \lambda_k = 1
$$

and the *affine hull* of these points, denoted as $aff({_1, x_2, \cdots, x_k})$, is all the affine combinations of them.

The **conic combination** of a set of points $x_1, x_2, \cdots, x_k$ in $\mathcal{R}^n$ is defined as

$$
\lambda_1 x_1 + \lambda_2 x_2 + \cdots + \lambda_k x_k, \lambda_i \geq 0, i = 1, ..., k
$$

and the *conic hull* of these points, denoted as $cone({_1, x_2, \cdots, x_k})$, is all the conic combinations of them.

The **convex combination** of a set of points $x_1, x_2, \cdots, x_k$ in $\mathcal{R}^n$ is defined as

$$
\lambda_1 x_1 + \lambda_2 x_2 + \cdots + \lambda_k x_k, \sum\_{i = 1}^k \lambda_k = 1, \lambda_i \geq 0, i = 1, ..., k
$$

and the *convex hull* of these points, denoted as $conv({_1, x_2, \cdots, x_k})$, is all the convex combinations of them.
