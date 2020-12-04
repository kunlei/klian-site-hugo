---
title: Projection
date: 2020-12-04
draft: false
tags: ["O.R."]
---

Projection is the process of, not surprisingly, projecting a polyhedron into a lower dimensional space.
First, let's first define a polyhedron:

$$
\begin{align}
  P = \\{(x, y) \in \mathcal{R}^{n_1} \times \mathcal{R}^{n_2} \, | \, Ax + By \geq b \\}
\end{align}
$$

A polyhedron can also be defined as the intersection of a finite number of half-spaces, and in the definition above, each $a_i x + b_i y \geq b_i$ essentially defines a half-space.
Now we can state that the projection of $P$ into the subspace of the $y$ variables is as follows:

$$
\begin{align}
proj_y(P) = \\{ y \in \mathcal{R}^{n_2} \, | \, \text{there exist } x \in \mathcal{R}^{n_1}\text{ such that } (x,y) \in P \\}
\end{align}
$$

To understand this, think of a polyhedron in the two dimensional space, its projection to either the $x$-axis or $y$-axis would be a line, and for a point to be included in the line, there must exist at least one point in the other dimension that together belongs to the original polyhedron.
Similarly, the projection of a cube into the $x-y$ space would be a square.