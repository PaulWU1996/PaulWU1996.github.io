---
layout: post
title: "distributed optimization"
subtitle: "First survey about this region"
date: 2021-01-26
author: "Peipei WU"
header-img: 
header-style: text
header-mask: 
tags: [distributed optimization]
mathjax: true
---

# Distributed Optimization Survey



___Sorry guys! I cannot complete this note today! I have to say that it is very difficult for me to understand it as a fresher. I hope I can complete this note tomorrow. Please still forward to read it!___



Hello everyone, today we are going to talk about distributed optimisation. After last week discussion with my supervisor [Wang Wenwu](https://surrey.ac.uk/people/wenwu-wang), we agreed that we need put our focus on distributed optimization or other regions such as distributed learning, distributed sensing. Unfortunately, the time I spent on distributed computing might not useless to our purpose, due to it is quite close to computer science that how can we achieve distributed computing, not how can we use it on our AV tracking system. From today, we will create some new series, today's topic distributed optimization is one of them. Of course, I will not give up the series about distributed computing. I will keep update it in future.

## 1 Introduction

Today we use one survey published on Annual Reviews in Control given by Tao Yang and his workmates. We will summary this survery in this note. However, this paper focuses on optimal coordination of distributed emerhy resources (DERs) in power system, it is different to our purpose, but we can still borrow some ideas about how to coordinate resource.

## 2 Distributed Optimization Definition

$N$ agents in networked system, each of agent has a local private convex objective function $f_i(x)$, where $x \in \mathbb{R^n}$ is the optimization variable. The objective of distributed optimization is to minimize a global objective function, which is a sum of the objective functions of all agents:



$$
\min_{x \in \mathbb R^n} \sum^{N}_{i=1} f_i(x)
$$



in a distributed manner by local computation and communication.

## 3 Preliminaries

In this section, we will discuss some basics including graph theory, convex analysis and convergence analysis. However, I will not give too much details but write in my own words to explain them. If possible or essential for confirmation report, I will write down  this later.

1. graph theory

   We use one set to contain nodes (or agents) and another set contains directed edge, then we can collect $G=(v,\xi)$. Please recall to the course, Data Structure,  all of us studied before. This is classical data structure directed graph. A difference is that there is no self loop in graph. It means there is no edge points to node itself, but node can access to its own information. Also we define an matrix $A$,which is one adjacency matrix to $G$ and it contains weights of edges in $G$. In same way, we can define the time-varying directed graph $G$, with differences that $\xi$ contains different time edges information and  nodes can be divided into in-neighbours and out-neighbours.

2. convex analysis

   Convex formats are given in this survey, I don't present them here again. If I have time, I will update here. Convex analysis aims on convex optimization which is a good method in optimization theory. __The most important point we need to memory is that the local optimal solution must be the global optimal solution in convex function. __If we model the whole distributed system in convex function, each node collect its own local convex function. If each node finds its local optimal solution, for global distributed system can find its global optimal solution.

3. convergence analysis

   There is nothing new to say about this quite familiar concept. Also the detail of it is given in survey, there are two different methods Q-linear and  R-linear. Both of their rates are exponential or geometric.

## 4 Basic distributed optimization algorithms

### 4.1 Discrete-time Algorithm

There are two branch in ths family, it depends on the step-sizes are diminishing or fixed. Most of them are relied on the thinking that the first-order algortihms based on consensus theory and the (sub)gradient method. If you have no idea with [consensus](https://en.wikipedia.org/wiki/Consensus_(computer_science)) like me, please have a look on this link.

#### 4.1.1 diminishing step-sizes

Each agent performs a consensus step and then a descent step along the local (sub)gradient direction of its own convex objective function. One updating function is given, it defines how to update agent's estimate of the optimal solution. (Nedic and Ozdaglar 2009) gave distributed (sub)gradient descent (DGD) algorithm with diminishing step-sizes. If you have no idea with [^DGD], please have a look on this [link](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=4749425), this paper contains details about DGD, I also expalin the core idea of DGD in footnote. Convergence is still slow due to diminishing step-size. Many efforts on covering this proble, recent researches give up diminishing step-size and replaced by fixed step-sizes.

#### 4.1.2 fixed step-sizes

Compare to diminishing step-sizes, fixed step-sizes algortihm can convergence quicker than diminishing step-sizes however Diminishing step-sizes algortihms can offer accurate estimate of global optimal solution. There are two famous and distinct fixed step-sizes algorithms. First is EXTRA [^EXTRA], the details of it is in [link](https://arxiv.org/pdf/1404.6264.pdf). Second one is DIGing [^DIGing], it is based on the combination of the distributed inexact gradient method and the gradient tracking technique. Also, to correct the error caused by the distributed gradient based algorithms based on the proportional-integral (PI) [^PI] .

### 4.2 Continuous-time algorithm

With the development of cyber-physical systems, continuous-time algorithms become true. There are two different groups, first-order gradient information or the second-order Hessian information, respectively.

#### 4.2.1 first order gradient based

Distributed PI algorithm, refer to PI in previous. It is motivated by a feedback mechanism and is a PI control. It ensures each agent follows its local gradient descent while consensus achieved among agents is ensured.

#### 4.2.2 second order gradient based

This family offers much more quicker convergence speed due to Hassian information. Zero-Gradient-Sum Algorithm (ZGS) [^ZGS] is one distinct method.

## Table Resource

Attached some tables offered by this survey. In the future, we will discuss each branch of algorithms.

![image-20210127184550235](/Users/paul/Documents/GitHub/PaulWU1996.github.io/img/in-post/image-20210127184550235.png)

![image-20210127184751142](/Users/paul/Documents/GitHub/PaulWU1996.github.io/img/in-post/image-20210127184751142.png)

![image-20210127184901979](/Users/paul/Documents/GitHub/PaulWU1996.github.io/img/in-post/image-20210127184901979.png)

![image-20210127184924852](/Users/paul/Documents/GitHub/PaulWU1996.github.io/img/in-post/image-20210127184924852.png)

[^DGD]:Every agent generates and maintains estimates of the optimal solution of the global optimization problem. These esti- mates are communicated (directly or indirectly) to other agents asynchronously and over a time-varying connectivity structure. Each agent updates his estimates based on local information concerning the estimates received from his immediate neigh- bors and his own cost function using a subgradient method.
[^EXTRA]: EXTRA algortihm contains two steps. The first step agent $i$ updates $x(i)$ based $x(i)=\sum_{j}^{N}w_{ij}x_j(0)-\alpha \bigtriangledown f_i(x_i(0))$, $\alpha$ is fixed step-size and the right of it is gradient of local objective function. The second step agent $i$ updates  $x(k+2)=x_i(k+1) + \sum_{j}^{N}w_{ij}x_j(k+1) - \sum_{j}^{N} \tilde w_{ij}x_j(k) -\alpha ( \bigtriangledown f_i(x_i(k+1)) - \bigtriangledown f_i(x_i(k)))$.  Compared to DGD, EXTRA uses estimates of optimal solution and gradients at the two previous iterations rather than only one previous iteration.
[^DIGing]: states are upfated as follows: $x_i(k+1)=\sum_{j}^{N}w_{ij}x_j(k)-\alpha y_i(k)$, $y_i(k)=\sum_{j}^{N}w_{ij}y_j(k)+ \bigtriangledown f_i(x_i(k+1)) - \bigtriangledown f_i(x_i(k))$. The variable $y_ i ( k )$ is used instead of the average gradient in first equation and it tracks the average gradient by employing dynamic average consensus.
[^PI]: each agent performs the following update: $x_i(k+1)=x_i(k)-v_i(k)- \alpha \bigtriangledown f_i(x_i(k))- \beta \sum_{j \in N_i} a_{ij}(x_i(k)-x_j(k))$, $v_i(k+1)=v_i(k)+\alpha \beta \sum_{j \in N_i}a_{ij}(x_i(k)-x_j(k))$. $x_i(k)$ is the local estimate of the global minimizer of agent $i$ at time step $k$.
[^ZGS]: It will update future.