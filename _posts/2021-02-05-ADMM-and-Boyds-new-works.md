---
layout: post
title: ADMM and Boyd's new works
subtitile: Study Note about ADMM
date: 2021-02-05
author: Peipei WU
tags: [distributed optimization]
mathjax: true
---

# Notes about ADMM Algorithm Family

# 1 Abstract

Hi, guys! Almost half week passed, I am going to published new note about ADMM and this note will be the report for this weekly meeting with my supervisor. In the first half part, I will introduce and explain ADMM algorithm. And second half, I will list Boyd's new works and summary them. Look forward any comments and suggestions! Also, I will complete the comment function this weekend, all readers can comment directly on this page and emails are still welcome. 



## 2 What is ADMM?

First we need to know what is ADMM? The full name of ADMM is Alternating Direction Method of Multipliers (ADMM). ADMM selects decomposition-coordination that means it divides the global problem into several small but easy to solve local problems first, then coordinate all those local problems up to get the global problem solution.  



### 2.1 General problem

Suppose we have general problem in form:


$$
\min f(x)+g(z) \\ Ax+Bz=c 
$$


where $ x \in R^s, Z \in R^{p \times s}, A \in R^{p \times s}, B \in R^{p \times n}, c \in R^p, f: R^s \to R, g: R^n \to R$. Among of them, $x$ and $z$ are optimal variables; $f(x)+g(z)$ is the objective function which is to search the minimum value; $Ax+Bz=c$ means $p$ different constraint conditions. Thus, we can write __Augmented Lagrangian__ in form as:


$$
L_p(x,z,y)=f(x)+g(z)+y^T(Ax+Bz-c)+(p/2)\||Ax+Bz-c\||^2_2
$$


$y$ is Lagrangian multiplier and $p$ is a penalty parameter and which is positive. The ___last term___ in __Augmented Lagrangian__ is a quadric penalty term. Based on these conditions, we can write ADMM iterative solution on this general problem in form:


$$
\begin{align}
& x^{k+1}= \arg \min_x L_p(x,z^k,y^k) \\
& z^{k+1}= \arg \min_z L_p(x^{k+1},z,y^k) \\
& y^{k+1}= y^k +p (Ax^{k+1}+Bz^{k+1}-c)
\end{align}
$$


We introduced scaled term $u=(1/p)y$  to rewrite them into this form (__Scaled Form__):


$$
\begin{align}
& x^{k+1}= \arg \min_x (f(x)+(p/2)\||Ax+Bz^k-c+u^k\||^2_2) \\
& z^{k+1}= \arg \min_z (g(z)+(p/2)\||Ax+Bz^k-c+u^k\||^2_2) \\
& u^{k+1}= u^k +Ax^{k+1}+Bz^{k+1}-c
\end{align}
$$


From __Scaled Form__, we can clearly see that each iteration is made up by 3 steps:

1. Solve minimum functions related to $x$ to update $x$
2. Solve minimum functions related to $z$ to update $z$
3. Update $z$



### 2.2 Convergence 

When conditions are satisfied:

1. $f$ and $g$ are closed, proper and convex
2. Lagrangian owns __saddle point__

ADMM can be proved convergence. I will not illustrate the prove process here, you can find it in appendix of ADMM survey written by Boyd in 2011. The sentence I want to say here is that ADMM will convergence slowly under high accurately demand, otherwise it can convergence sufficiently under middle accurately demand.  



### 2.3 Consensus

First, we define the consensus problem in form:


$$
\min \sum_{i=1}^N f_i(z)+g(z)
$$


If we change the first term $\sum_{i=1}^N f_i(z)$ into separable objective function $\sum_{i=1}^N f_i(x_i)$ and add new constraints, the consensus problem can be written in new form:


$$
\begin{align}
& \min \sum_{i=1}^N f_i(x_i)+g(z) \\
& x_i-z=0, i=1,\cdots, N
\end{align}
$$


Thus, it is clearly that variable $x_i$ in each local objective functions must be same with global variable $z$ . We can use ADMM to solve consensus problem and its iteration solution is in form:


$$
\begin{align}
& x^{k+1}= \arg \min_{x_i} (f_i(x_i)+(p/2)\|x_i+z^k+u_i^k\|^2_2) \\
& z^{k+1}= \arg \min_z (g(z)+(Np/2)\|z- \overline x^{k+1} - \overline u^k\|^2_2) \\
& u^{k+1}= u^k +x^{k+1}+z^{k+1}\\
&\overline x=(1/N)\sum_{i=1}^N x_i\\
&\overline u=(1/N)\sum_{i=1}^N u_i\\
\end{align}
$$


The most significant benefit in this iteration form is that we can solve it in parallel or distributed way. When we compute and update variable $x$ and $u$, they can be refreshed parallelly due to it is independent to other variables. In other words, if we want to calculate $x$, no more variable values are waited, $z^k$ and $u_i^k$ are collected from previous iteration, thus no need to wait updated value in current iteration. Same to variable $u$. In these 3 steps, only second step z-updating needs to wait first step finished.



### 2.4 Distributed Implementation

In this section, two different description model are discussed. MPI and MapReduce are two very famous models to represent distributed system.



___MPI___

Intialize N processes, along with $x_i$ , $u_i$ , $r_i$ , $z$

__Repeat__

1. ​       Update $u_i=u_i+r_i$
2. ​       Update $x_i= \arg \min_x (f_i(x)+(p/2)\|\| x-z+u_i \|\|^2_2)$
3. ​       Let $w=x_i+u_i$ and $t= \|\|r_i\|\|^2_2$
4. ​       Allreduce[^1] $w$ and $t$ 
5. ​       Let $z^{prev}=z$ and update $z = prox_{g,Np}(w/N))$
6. ​       __exit if__  $p\sqrt{N}\|\|z-z^{precv}\|\|_2 \le \xi^{conv}$ and $\sqrt{t} \le \xi^feas$
7. ​       Update $r_i=x_i-z$



___MapReduce( Recommend!!!!)___

__Function__ map(key $i$, dataset $D_i$):

1.  Read $(x_i, U_i, \hat z)$ from distributed database
2. Compute $z= \arg \min_z (g(z)+(Np/2)\|z-\hat z/N\|^2_2)$
3. Update $u_i=u_i+x_i-z$
4. Update $x_i= \arg \min_x (f_i(x)+(p/2)\|x-z+u_i\|^2_2)$
5. Emit(key ___CENTRAL___, record($x_i$,$u_i$))

__EndFunction__



__Function__ reduce(key ___CENTRAL___, records($x_1$,$u_1$),$\cdots$,($x_N$,$u_N$))

1. Update $\hat z = \sum_{i=1}^N (x_i+u_i)$
2. Emit(key $j$, record($x_j$,$u_j$,$z$)) to distributed database for $j=1,\cdots, N$

__EndFunction__



## 3 Recent Boyd's works

In this section, I will list recent Boyd's works based on the year of published (this work is updating and not completed). Not too much details are included, but I will introduce the main contributions and the problems those publications focused.



__2021__

___

| Work                                                 | Focus problem                                                | Contribution                                                 |
| ---------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Automatic Repair of Convex Optimization Problems[^2] | Given an infeasible, unbounded, or pathological convex optimization problem.  __How to make the smallest change to  make the problem's parameters become solvable?__ | Proposed heuristic for approximately solving this problem that is based on the penalty method. To find the closest solvable optimization convex problems, two loops are included in algorithm. One is updating parameters based on penalty method and another one is solving sub-problem based via proximal gradient method. |
| Covariance Prediction via Convex Optimization        | __The problem of predicting the covariance of a zero mean Gaussian vector, based on another feature vector.__ | Authors observed that covariance predictor can be iterated, however how to choose the sequences of predictors is still a question. |

__2020__

___

| Work                                                         | Focus problem | Contribution                                   |
| ------------------------------------------------------------ | ------------- | ---------------------------------------------- |
| Embedded Convex Optimization for Control[^3]                 |               | This talk summaries advances in this 10 years. |
| A Distributed Method for Fitting Laplacian Regularized Stratified Models[^4] |               |                                                |
|                                                              |               |                                                |
|                                                              |               |                                                |
|                                                              |               |                                                |



## 3. EndNote

Today, I introduced and discussed ADMM algorithm. And I list some literatures published by Boyd and his team. Unfortunately, I did not complete whole literature survey of his works. Thus, I will keep updating this note until all Boyd's works summarized here.  Thank you for your reading!

[^1]: __AllReduce__ is an operation that reduces the target arrays in all processes to a single array and returns the resultant array to all processes.
[^2]: <https://web.stanford.edu/~boyd/papers/auto_repair_cvx.html>
[^3]: <https://web.stanford.edu/~boyd/papers/cdc_20.html>
[^4]: <https://web.stanford.edu/~boyd/papers/strat_models.html>