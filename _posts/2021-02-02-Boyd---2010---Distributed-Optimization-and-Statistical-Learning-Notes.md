---
layout: post
title: Boyd - 2010 - Distributed Optimization and Statistical Learning Notes
subtitile: Basics of distributed optimization
date: 2021-02-02
author: Peipei WU
tags: [distributed optimization]
mathjax: true
---

# Study Note (1)

Hi, today I'm going to summary my study notes about distributed optimization and statistical learning by Boyd. Two chapters are included. I will finish this book before weekly meeting with my supervisor. Look forward for any suggestion and comment! <p.wu@surrey.ac.uk>.

## 1 Precursors

1. Dual Ascent

   Consider the quality-constrained convex optimization problem, use Lagrangian to describe it and dual function of Lagrangian can be collected. Parameter $y$  in dual function is the dual var or Lagrange multiplier. Thus, dual problem is to find the minimum of dual function. __Primal optimal point can be recovered from a dual optimal point. __ In Dual Ascent, dual problem is solved by using gradient ascent. There are two steps in dual ascent. First step (__x-minimization step__) is to find a minimizer of Lagrange (dual optimal point) at the iteration $k+1$ based Lagrange function with primal optimal point in previous iteration $k$, second step (__dual var update__) is to update the primal optimal based on gradient. When dual function of Lagrange function is not differentiable, sub-gradient is used to replace gradient.

   ___Major benefit of this method is that it can lead to a decentralizes algorithm in some cases!___

2. Dual Decomposition

   Suppose the objective $f$ is separable and partitioning the matrix $A$ is existed. Then Lagrange function can be represented in another form. __This innovation results that the first step x-minimization step can be run in parallel!__ In the general case, each iteration of the dual decomposition method requires a _broadcast_ and a _gather_ operation. In the dual update step the equality constraint residual contributions are collected (gathered). Once the (global) dual var is computed, it must be distributed (broadcast) to the processors that carry out the individual x-minimization steps.

3. Augmented Lagrangians and the Method of Multipliers

   Augmented Lagrangian methods were developed in part to bring robustness to the dual ascent method, and in particular, to yield convergence without assumptions like strict convexity or finiteness. Lagrangian function is written in another format and parameter $p$ is introduced as penalty parameter. The benefit of including the penalty term is that dual function can be shown to be differentiable under rather mild conditions on the original problem. ___The greatly improved convergence properties of the method of multipliers over dual ascent comes at a cost.___



## 2 Alternating Direction Method of Multipliers

ADMM is an algorithm that is intended to blend the decomposability of dual ascent. The problem forms in:


$$
minimize \space  f(x)+g(z)\\

subject \space to \space Ax+Bz=C
$$


The var here has been split into two parts as $x$ and $z$. The algorithm is very similar to dual ascent and the method of multipliers: it consists of an x-minimization, a z-minimization step, and a dual variable update.

__Scale Form:__ Define the residual form $r=Ax+Bz-c$ and scaled dual var is introduced. Usually it is shorter than unscaled form.

__Convergence:__ Two assumptions:  The (extended-real-valued) functions are closed, proper, and convex; The unaugmented Lagrangian $L_0$ has a saddle point. Then, there are: ___Residual convergence___,  the iterates approach feasibility; ___Objective convergence___, the objective function of the iterates approaches the optimal value; ___Dual variable convergence___.

__Termination criterion:__ primal and dual residuals must be small.

__Extensions and Variations:__  

1.  _Varying Penalty Parameter_, A standard extension is to use possibly different penalty parameters for each iteration.
2.  _More General Augmenting Terms_, Another idea is to allow for a different penalty parameter for each constraint, or more generally, to replace the quadratic term.
3. _Over-relaxation_, In the z-updates and y-updates, the quantity can be replaced and relaxation parameter is introduced.



