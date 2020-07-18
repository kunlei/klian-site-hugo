---
title: Capacitated Facility Location Problem - Modeling
date: 2020-07-17
draft: false
tags: ["optimization", "Benders decomposition"]
---

This post will examine a typical application of Benders decomposition, the capacitated facility location problem.
We'll first give a brief description of the problem, followed by the formal mathematical model.
The testing instances used in this and future posts will be introduced in the end.

### Problem description

In the capacitated facility location problem, there are $m$ candidate facilities to serve the demands $d_j$ from $n$ customers.
A facility $i$ has limited capacity $q_i$ and incurs an operational cost of $f_i$ if it's opened.
The cost of serving customer $j$ from facility $i$ is denoted as $c\_{ij}$.
The problem is to decide which facilities to open and the amount of demands satisfied from facility $i$ for customer $j$, with the objective of minimizing the total cost.

### Mathematical model

Define the following two decision variables:

+ $x\_{ij}$: continuous variable that represents the amount of demands from customer $j$ that is satisfied by facility $i$.
+ $y_i$: binary variable equals 1 if facility $i$ is opened, 0 otherwise.

The model can be formulated as

$$
\begin{align}
\mbox{min.} \quad & \sum_{i = 1}^{m} \sum\_{j = 1}^n c\_{ij} x\_{ij} + \sum\_{i = 1}^m f_i y_i \newline
\mbox{s.t.} \quad & \sum\_{i = 1}^{m} x\_{ij} \geq d\_j, \ \forall j = 1,\cdots, n \newline
& \sum\_{j = 1}^n x\_{ij} \leq q_i y_i, \ \forall i = 1, \cdots, m \newline
& x\_{ij} \geq 0, \ \forall i = 1, \cdots, m, j = 1, \cdots, n  \newline
& y \in \\{0, 1\\}, \ \forall i = 1, \cdots, m
\end{align}
$$

In the formulation, the objective is to minimize the total operational costs of all open facilities and all the service costs.
The first constraints make sure that all customer demands are satisfied.
The second constraints guarantees that only open facilities can serve customers.
The last two constraints specify the variable types.

### Testing

There exist lots of benchmarking instances in the literature, some of them can be found here: 
http://people.brunel.ac.uk/~mastjjb/jeb/orlib/capinfo.html

In future posts, I'll illustrate how to solve the model directly using CPLEX Java API.
I'll also show how to decompose the problem so that Benders' approach can be applied.
Moreover, the Benders' decomposition will be demonstrated in two ways: a manual approach that solve a main problem and a subproblem iteratively; 
and an automatic Benders' approach enabled by CPLEX starting from version 12.8.0.
