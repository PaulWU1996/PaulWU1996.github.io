---
layout: post
title: "Particle Flow Summary   1"
subtitle: "Particle Flow For Nonlinear Filters"
date: 2021-01-21
author: "Peipei WU"
header-img: 
header-style: text
header-mask: 
tags: [particle flow]
mathjax: true
---

# Study Notes About Particle Flow For Nonlinear Filters

Hello there! Today I'm going to discuss and present Duam's excellent work about Particle Flow For Nonlinear Filters. Thanks to Dr. [Liu Yang](https://sanglongbest.github.io) again, this work is suggested by himself and very good for freshers to particle flow as me. Also, this note is one work record for myslef and will be discussed with my superviosr Professor [Wang Wenwu](https://surrey.ac.uk/people/wenwu-wang) this week. Let's have a look on this paper.

## Contribution

1. Do not resample particles
2. Do not rely on proposal density
3. Compute the log of the unnomalized density using particle flow
4. Do not use importance sampling or any MCMC

## Derivation of PDE

Compute a flow of particles induced by the following flow of the conditional unnormalized probability density of $x$:

$$
\log p(x,\lambda) = \log g(x) + \lambda \log h(x)
$$

$x$ is the d-dimensional state vector, $g(x)$ denotes the prior unnormalized probability density of $x$, $h(x)$ is likelihood. In other words, we can regard $h(x)$ as measurement likelihood in our AV tracking system, such as certainty of vision or auditory detection (darknet, mask-rcnn and etc.). $x$ is more similar to system  description, we will use this to represent how will algorithm to assign particles. Variable $\lambda$ is one real number parameter from 0 to 1, as described by author, they said that this parameter is similar to time but not really. Relate to equation given previously, I prefer regard it as one weight to represent the how important of measurement likelihood. For instance,  when  $\lambda = 0$ , $p(x,\lambda)$ is equal to the prior density, whereas  $\lambda = 1$ , the function is equal to the desired posteriori density of $x$ conditioned on all measurements (vision detection and audio detection) up to and including the current time. To simplfy it, it seems like to add measurements result on prediction given by previous frame. In the mean time, $\log$ is introduced to avoid singularities or any possible problems.

Suppose there exists a continuous flow of particles induced by probabilty with following dynamics:

$$
\frac{dx}{d\lambda} = f (x,\lambda)
$$

Therefore, the probability density of $x$ will obey the Fokker-Planck equation throught this flow:

$$
\frac{\partial p(x,\lambda)}{\partial \lambda}= -Tr[\frac{\partial (p(x,\lambda)f(x,\lambda))}{\partial x}]
$$

Unfortunately I am not student in physics, using my own words to explain Fokker-Planck is that this is a nature rule of particles assignments after effected by ramdom force. If anybody knows details of it, please add them in comment.

If there is no process noise in the flow of $x$ , it is possible to collect PDE for particle flow with non-zero diffusion. Based on definition of $p(x,\lambda)$ :
$$
\frac{\partial \log p(x,\lambda)}{\partial \lambda}=\log h(x)
$$

Combine above equation we can obtain a result:
$$
\log h(x)p(x,\lambda)=-p(x,\lambda)Tr[\frac{\partial f}{\partial x}]-\frac{\partial p}{\partial x}f
$$

Assuming $p(x,\lambda)$ always exists, the desired PDE:
$$
\log h=-div(f)-\frac{\partial \log p}{\partial x}f
$$

Put this PDE into divergence form by definning  $q(x,\lambda)=p(x,\lambda)f(x,\lambda)$:
$$
div(q)=\eta=-p(x,\lambda)\log h(x)
$$
In which, $div$ denotes divergence. [^1] and  [^2] offer some distinct methods to solve PDEs upon.

**In this part, we presents some work about PDEs and it will help to induce how will we assign the particles in flow.**

## Separation of Variables

Furthermore,we assume that the prior density and the likelihood are Gaussian densities, we can obatin a result for our particle flow corresponding to Bayes' rule:
$$
\frac{dx}{d\lambda}=A(\lambda)x+b(\lambda)
$$
in which
$$
A=-\frac{1}{2}PH^T(\lambda HPH^T+R)^{-1}H
$$

$$
B=(I+2\lambda A)[(I+\lambda A)PH^TR^{-1}z+A\overline x]
$$
$P$ is the covariance matrix of prediction errior in $x$ for the Gaussan prior density, $H$ is the measurement matrix $(z=Hx+v)$, R is the covariance matrix of measurement noise $(v)$. $\overline x$ denotes the predicted value of x, corresponding to mean value of prior Gaussian density.

**Don't forget that**$\frac{dx}{d\lambda}$ **is what we want to obatin in previous, it is the probability to induce particle flow. Also, what we talk in this part means solving about PDEs can be used exp probability densities rather than gaussian densities, this can boost speed of algorithm!**

## Direct Integration 

Suppose exact solution of PDE with same dimension is collected, also details are given in section derivation of PDE:
$$
div(\tilde q)=\eta
$$
Of course, we can compute $A$ and $b$ by exploting $H$, $R$ and $P$. Combining those up:
$$
div(q-\tilde q)=\eta - \tilde \eta
$$
Based on this, we can compute exact solution by integrating selected non-zero component:
$$
q_j=\tilde q_j + \int^{x_j}\eta(x,\lambda)- \tilde \eta(x,\lambda)dx_j
$$
for any $j$ of our choice ($q_j-\tilde q_j$ is non-zero component), and $q_i=\tilde q_i$ for $i \not =j$. This freedom offers us convience to solve PDE and accelerates up. For instance, if $\eta - \tilde \eta$ is approximately constant in $x_j$ , then:
$$
q_j \approx \tilde q_j + x_j (\eta-\tilde \eta)
$$

If we use higher order taylor series in $x_j$ to expand, a better approximation point would be obtained. And measurements likelihood $h(x)$ is known exactly, in contrast $g(x)$ is very difficult to get. This point can help us to solve PDE much more quickly.

**To emphasise that this section makes particle flow to nonlinear system, not only Gaussian densities. In other words, particle flow can be used in real world!**

## Summary Thinking about Partical Flow in Ancient Time

__We want to obtain best particles assignments, thus the unnormalized density of particle $x$ is required. However, we don't want to multiply two elements, log-homotopy is introduced to handle. Based above, we can find an equation to describe desired density. Then, we develop it into nonlinear system based Bayes' rule and two functions $A$ and $b$ are created. Moreover, Gaussian density is replaced by exp density, this boosts up speed. Next, to solve PDE, one method is presented, in which we do only focus on measurements likelihood rather than prior density, this makes easier than past time. Poisson's equation is one problem existed in this period and we did not discuss it in this note. New version of particle flow in 2010s can cover them.__

## Endnote

Thank you for your reading. This is my first studying note about particle flow with details. We discussed the particle flow in ancient time (before 2010). With development of it, more new versions particle flow are created, with robustly and efficiently. In next few days, I am going to introduce rest particle flow and present how to use them in AV tracking task. Also, I will write down some distributed computing basics here in same time.

## Referrence

[^1]:Fred Daum, Jim Huang and AJ Noushin, “Exact particle flow for nonlinear filters,” Proceedings of SPIE Conference, Orlando Florida, April 2010.
[^2]: Fred Daum and Jim Huang, “Exact particle flow for nonlinear filters: seventeen dubious solutions to a first order linear underdeteremined PDE,” Proceedings of IEEE Conference on Signals, Systems & Computers, Asilomar California, November 2010.







