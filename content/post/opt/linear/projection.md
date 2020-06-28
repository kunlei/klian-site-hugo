---
title: Fourier-Motzkin Elimination
date: 2020-06-25
draft: false
tags: ["linear programming"]
---

The Fourier-Motzkin elimination (FME) is a topic I glanced over in my learning of linear programming.
However, I recently read a in-depth treatment of FME and it is really an intriguing technique to learn optimization theories.

## How it works

In short, FME is used to eliminate variables from a set of inequalities.
Let's say we have a column variable $\mathcal{x}$ that is defined in the $\mathcal{R^n}$ space, and is subject to $Ax \ge b$, FME can eliminate one or more variables from $x$, thereby projecting $x$ into the corresponding lower dimensional space.
To help elaborate the process, let's assume rows in $Ax \ge b$ can be put into two categories, one in which the coefficient associated with $x_1$ is positive, and the other negative.

$$
  a_{i1} x_1 + a\_{i2} x_2 + \cdots a\_{in} x_n \geq b_i, \forall i \in M_i \newline
  -a\_{j1} x_1 + a\_{j2} x_2 + \cdots a\_{jn} x_n \geq b_j, \forall j \in M_j \newline
$$

Let's further assume that both $a_{i1}$ and $a\_{j1}$ are positive numbers, and putting the coefficients associated with $x_1$ into 1 results in

$$
  x_1 + \frac{a\_{i2}}{a\_{i1}} x_2 + \cdots + \frac{a\_{in}}{a\_{i1}} x_n \geq \frac{b_i}{a\_{i1}},\forall i \in M_i \newline
  -x_1 + \frac{a\_{j2}}{a\_{j1}} x_2 + \cdots + \frac{a\_{jn}}{a\_{j1}} x_n \geq \frac{b_j}{a\_{j1}}, \forall j \in M_j \newline
$$

Then we have

$$
\frac{b_i}{a\_{i1}} - \frac{a\_{i2}}{a\_{i1}} x_2 - \cdots - \frac{a\_{in}}{a\_{i1}} x_n \leq x_1 \leq
\frac{a\_{j2}}{a\_{j1}} x_2 + \cdots + \frac{a\_{jn}}{a\_{j1}} x_n - \frac{b_j}{a\_{j1}}, \ \forall i \in M_i,\  j \in M_j
$$

which translates to

$$
(\frac{a\_{i2}}{a\_{i1}} + \frac{a\_{j2}}{a\_{j1}}) x_2 + \cdots + (\frac{a\_{in}}{a\_{i1}} + \frac{a\_{jn}}{a\_{j1}})x_n \geq (\frac{b_i}{a\_{i1}} + \frac{b_j}{a\_{j1}}), \ \forall i \in M_i,\  j \in M_j
$$

Note that the above set of inequalities has eliminated the $x_1$ variable when compared to the original set, but has more constraints!

Following the same process, it is possible to eliminate all variables from the left hand side and eventually we end up with the final set of inequalities like below:

$$
0 \geq v_k
$$

If any of the $v_k$ is positive, it implies that the original set of inequalities has no solution!

## Example

Now let's look at an example:

$$
x_1 + 2x_2 + 3x_3 \geq 5 \newline
x_1 + 3x_2 - 4x_3 \geq 4 \newline
-x_1 - x_2 - 2x_3 \geq 2 \newline
-x_1 - 4x_2 + 2x_3 \geq 6 \newline
-x_1 + 3x_2 - 5x_3 \geq 3
$$

Eliminating $x_1$ results in

$$
x_2 + x_3 \geq 7 \newline
-2x_2 + 5x_3 \geq 11 \newline
5x_2 - 2x_3 \geq 8 \newline
2x_2 - 6x_3 \geq 6 \newline
-x_2 - 2x_3 \geq 10 \newline
6x_2 -9 x_3 \geq 7 \newline
$$

Making the coefficients in the first column 1 or -1 and rearranging rows, we  have

$$
x_2 + x_3 \geq 7 \newline
x_2 - \frac{2}{5}x_3 \geq \frac{8}{5} \newline
x_2 - 3x_3 \geq 3 \newline
x_2 - \frac{3}{2}x_3 \geq \frac{7}{6} \newline
-x_2 - 2x_3 \geq 10 \newline
-x_2 + \frac{5}{2}x_3 \geq \frac{11}{2} \newline
$$

Eliminating $x_2$ results in

$$
x_3 \geq \frac{25}{7} \newline
x_3 \geq \frac{71}{21} \newline
x_3 \geq \frac{20}{3} \newline
-x_3 \geq 17 \newline
-x_3 \geq \frac{29}{6} \newline
-x_3 \geq \frac{13}{5} \newline
-x_3 \geq \frac{67}{21} \newline
$$

Finally eliminating $x_3$ results in

$$
0 \geq \frac{25}{7} + 17 > 0\newline
0 \geq \frac{25}{7} + \frac{29}{6} > 0\newline
0 \geq \frac{25}{7} + \frac{13}{5}  > 0\newline
0 \geq \frac{25}{7} + \frac{67}{21}  > 0\newline
0 \geq \frac{71}{21} + 17  > 0\newline
0 \geq \frac{71}{21} + \frac{29}{6} > 0\newline
0 \geq \frac{71}{21} + \frac{13}{5} > 0\newline
0 \geq \frac{71}{21} + \frac{67}{21} > 0\newline
0 \geq \frac{20}{3} + 17 > 0\newline
0 \geq \frac{20}{3} + \frac{29}{6} > 0\newline
0 \geq \frac{20}{3} + \frac{13}{5} > 0\newline
0 \geq \frac{20}{3} + \frac{67}{21} > 0\newline
$$

It's clear that the system of inequalities have no solution as $0 > 0$ is a false statement.

## Projection

The elimination of variables for a system of inequalities effectively projects a polyhedron defined by the system into lower dimensional space.
This can be used to solve linear programming problems, which I will elaborate in separate post.
It's unfortunate that this method is not applicable for practical problems as the number of inequalities grows exponentially as we eliminate more and more variables.
