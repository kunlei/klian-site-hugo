---
title: Solving Linear Programs using Projection
date: 2020-07-05
draft: false
tags: ["projection"]
---

Having used column generation, both in my graduate study and working experience, to solve large scale linear programming problems with many variables, I am quite familiar with the simplex algorithm.
It turns out that there is another way, although not necessarily fast theoretically or practically, to solve linear programs using projection.
I feel the reasoning is intriguing as well as intuitive to understand, which is detailed below.

## Projection with Fourier-Motzkin Elimination

I explained in an earlier post the inner workings of using projection to eliminate variables from a system of linear inequalities of the form:

$$\mathbf{Ax} \geq \mathbf{b}$$
 
where $\mathbf{A}$ is a $m \times n$ matrix, $\mathbf{x}$ is a column vector of size $n$, and $\mathbf{b}$ is a column vector of size $m$.
Using projection, specifically the Fourier-Motzkin elimination method, to eliminate all $\mathbf{x}$ variables results in a system of inequalities with all zeros on the left hand side, and constants on the right hand side.

$$
0 \geq d_k, \ k \in \\{1, ..., K\\}
$$

Note that any positive $d_k$ indicates infeasibility of the system of inequalities.
The idea of projection makes it extremely easy to interpret the theorem of alternatives, one that puzzled me for a long time, not about how it works and more about its significance.

## Application to linear programming

It's not surprising to see that $\mathbf{Ax} \geq \mathbf{b}$ defines the feasible space for a linear programming problem in general form:

$$
\begin{align}
\text{min.} \quad& \mathbf{c'x} \newline
\text{s.t.} \quad & \mathbf{Ax} \geq \mathbf{b}
\end{align}
$$

Now we introduce a new variable, $y$, to represent the objective value and let $y \geq \mathbf{c'x}$.
The above linear program can be rewritten as 

$$
\begin{align}
\text{min.} \quad& y \newline
\text{s.t.} \quad & y - \mathbf{c'x} \geq 0 \newline
& \mathbf{Ax} \geq \mathbf{b}
\end{align}
$$

Using Fourier-Motzkin elimination to project out all $x$ variables results in

$$
\begin{align}
\text{min.} \quad& y \newline
\text{s.t.} \quad & y  \geq d_k, k \in \\{1, ..., K\\} \newline
& 0 \geq d_k, k \in \\{K + 1, ..., L \\}
\end{align}
$$

It's then clear that the optimal objective value is

$$
\text{max}\\{d_k | k = 1, ..., K \\}
$$

Unfortunately, the number of constraints $L$ could be very large, preventing it to solve linear programming problems of practical sizes.
Still, the idea of projection can help understand concept like duality and the theorem of alternatives, which I will elaborate in future posts.
