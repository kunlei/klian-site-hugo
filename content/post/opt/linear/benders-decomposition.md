---
title: Benders Decomposition
date: 2020-07-12
draft: false
tags: ["optimization", "decomposition"]
---

Benders decomposition is a potent, albeit advanced, technique to attack optimization problems with certain special structures.
Specifically, it's applicable when a model possesses the following format:

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

where $K$ is the number of possible multipliers that enable this projection.

Given that $u \geq 0$, the first constraints can be subsumed into two categories based on whether $u = 0$ or $u \gt 0$ (which can be further rescaled into $u = 1$)

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

### Projection cone

Now that we have a model with humongous number of constraints, which is not necessarily exciting to include all at once.
Fortunately, the $K$ sets of multipliers possess some special attributes, namely, they are all solutions to the following system:

$$
\begin{align}
\mathbf{v'A} + \mathbf{w'I} = u\mathbf{c'} \newline
u \geq 0 \newline
\mathbf{v} \geq 0 \newline
\mathbf{w} \geq 0 \newline
\end{align}
$$

They are also called **projection cone**

$$
\begin{align}
C_x(P) = \\{(u, \mathbf{v}, \mathbf{w}) | \mathbf{v'A} + \mathbf{w'I} - u\mathbf{c'} = 0, (u, \mathbf{v}, \mathbf{w}) \geq 0\\}
\end{align}
$$

It turns out that not all solutions to the above system need to be generated - only the extreme rays are needed.
Again, depending on whether $u = 0$ or $u > 0$ ($u = 1$ after proper scaling), the extreme rays can be generated in two scenarios:

+ if $u = 1$, $\mathbf{v}$ are the extreme points of the polyhedron $\\{\mathbf{v} | \mathbf{v'A} \leq \mathbf{c}, \mathbf{v} \geq 0\\}$
+ if $u = 0$, $\mathbf{v}$ are the extreme rays of the recession cone $\\{\mathbf{v} | \mathbf{v'A} \leq 0, \mathbf{v} \geq 0\\}$

## Delayed constraint generation

Now let's step back and check what we've achieved so far:

+ We've transformed the original formulation with both the $\mathbf{x}$ and $\mathbf{y}$ variables into one with only the $\mathbf{y}$ variable.
+ The new formulation has many more constraints that are determined by the extreme rays of a cone involving only the $\mathbf{x}$ variables.

In other words, we've effectively decomposed the original problem into two subproblems, one we refer to as the main problem, and the other as the subproblem.
Since it's not practical or even necessary to include all the constraints, we can employ a small portion of the constraints and solve a relaxed main problem first.
Then by identifying the constraints that the resulting main problem solution violates, we can iteratively add more constraints on the fly, this is a technique called as delayed constraint generation.

### Subproblem

Let's formally define the subproblem:

$$
\begin{align}
\mbox{max.} \quad & \mathbf{f'y} + \mathbf{v'(b - By)} \newline
\mbox{s.t.} \quad & \mathbf{v'A} \leq \mathbf{c} \newline
& \mathbf{v} \geq 0
\end{align}
$$

With a given $\mathbf{y_0}$, the main problem can be solved first to get its optimal value $z_0$.
By solving the subproblem, we obtain the optimal solution $\mathbf{v_0}$ and the corresponding optimal value $\mathbf{f'y\_0} + \mathbf{v'\_{0}(b - By_0)}$.
There exist three scenarios:

+ the subproblem has an optimal value and $z < \mathbf{f'y\_0} + \mathbf{v'\_{0}(b - By_0)}$, in this case, we add the constraint $z \geq \mathbf{f'y} + \mathbf{v'\_{0}(b - By)}$ to the main problem
+ the subproblem is unbounded, in this case, we add the constraint $\mathbf{v'\_0(b - By)} \leq 0$ to the main problem.
+ the subproblem is infeasible, terminate the main problem.

### Lower bound and upper bound

Note that each optimal solution to the relaxed main problem provides a lower bound to the original formulation.
On the other hand, the optimal value of the subproblem gives a valid upper bound.
To see this, notice that the subproblem is equivalent to

$$
\begin{align}
\mbox{min.} \quad & \mathbf{f'y} + \mathbf{c'x} \newline
\mbox{s.t.} \quad & \mathbf{Ax}  \geq \mathbf{b} - \mathbf{By}\newline
& \mathbf{x} \geq 0 \newline
\end{align}
$$

Therefore, the optimal dual solution of the subproblem, together with the $\mathbf{y}$ value, provides a feasible solution to the original problem, which is also a valid upper bound on the relaxed main problem.

## Benders decomposition workflow
