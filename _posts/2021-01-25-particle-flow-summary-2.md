---
layout: post
title: "particle flow summary 2"
subtitle: "Particle Flow Development After 2011"
date: 2021-01-25
author: "Peipei WU"
header-img: 
header-style: text
header-mask: 
tags: [particle flow]
mathjax: true
---

# Summary of Particle Flow Development after 2011

## Introduction

In previous note, we have discussed about Professor Duam's works before 2011. In this note, we will discuss about the advanced particle flow with its development after that year. There are 2 papers are going to be discussed based on their publication year.

## Coulomb's particle flow for nonlinear filters

Compared to previous work, the flow of particles induced by the following flow of the conditional normalized probability density of $x$:


$$
\log p(x,\lambda) = \log g(x) + \lambda \log h(x)-\log K(\lambda)
$$


$K(\lambda)$ is a function normalizes the density $p(x,\lambda)$ along the flow.  Same, the PDE changes:


$$
\frac{\partial \log p(x,\lambda)}{\partial \lambda}=\log h(x)-\frac{\partial \log K(\lambda)}{\partial \lambda}
$$


The probability density of $x$ is still based on Fokker-Planck equation without any modification. Combining them up:


$$
[\log h(x)-\frac{\partial \log K(\lambda)}{\partial \lambda}]p(x,\lambda)=-p(x,\lambda)Tr[\frac{\partial f}{\partial x}]-\frac{\partial p}{\partial x}f
$$


Also PDE divergence form  $q(x,\lambda)=p(x,\lambda)f(x,\lambda)$:


$$
div(q)=\eta (x,\lambda)=-p(x,\lambda)[\log h(x)-\frac{\partial \log K(\lambda)}{\partial \lambda}]
$$


The normalization factor $K(\lambda)$ is defined as follows: 


$$
K(\lambda) = \int g(x)h^\lambda(x)dx
$$

$$
\frac{\partial \log K(\lambda)}{\partial \lambda}=\frac{\int p(x)\log h(x)dx}{\int p(x)dx}=E[\log h(x)]
$$


Thus PDE can be written as:


$$
div(q)=-p(x,\lambda)[\log h(x)-E[\log h(x)]]
$$


## Particle flow with non-zero diffusion for nonlinear filters

In previous works, $p(x,\lambda)$ and $\eta(x,\lambda)$ are used for solving $f(x,\lambda)$, and which is going to provide ODE for the desired flow of particles corresponding to Bayes' rule. And all of those are without non-zero process noise (non-zero diffusion). We can re-define the PDE, $Q$ is the covariance matrix of the process noise:


$$
p[\log h - \frac{d \log K}{d\lambda}]=-div(pf)+\frac{1}{2}div[Q(x)\frac{\partial p}{\partial x}]
$$
Same divergence of $pf$: $div(pf)=pdiv(f)+\frac{\partial p}{\partial x}f$, combining above two equations:


$$
\log h - \frac{d \log K}{d\lambda}=-div(f)-\frac{\partial \log p}{\partial x}f+\frac{1}{2}div[Q(x)\frac{\partial p}{\partial x}]
$$


If $div(f)$ equals to difussion term:


$$
div(f)=\frac{1}{2p}div[Q(x)\frac{\partial p}{\partial x}]
$$


Then PDE simplifies into:


$$
\log h - \frac{d \log K}{d\lambda}=-\frac{\partial \log p}{\partial x}f
$$
This can be solved by Moore-Penrose inverse:


$$
f=-[-\frac{\partial \log p}{\partial x}]^\sharp(\log h-\frac{d\log K}{d \lambda})\\

L^\sharp=\begin{cases}
\frac{L^T}{LL^T} & for LL^T \not =0\\
0 & otherwise
\end{cases}
$$

## EndNote

There are two papers are discussed today. We give the equations in details. We can expolit particle flow into non-zero diffusion tasks, this makes particle flow cann be used for tasks in real world. But I have to say that I still fresh to particle flow, I cannot explain particle flow very well currently. Next note about particle flow, we will talk about two very important works, Stochastic Particle Flow and Gromov’s method. 

