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

Each agent performs a consensus step and then a descent step along the local (sub)gradient direction of its own convex objective function. One updating function is given, it defines how to update agent's estimate of the optimal solution. (Nedic and Ozdaglar 2009) gave distributed (sub)gradient descent (DGD) algorithm with diminishing step-sizes