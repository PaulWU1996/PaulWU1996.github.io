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

Hello there! Today I'm going to discuss and present Duam's excellent work about Particle Flow For Nonlinear Filters. Thanks to Dr. [Liu Yang](https://sanglongbest.github.io) again, this work is suggested by himself and very good for freshers to particle flow as me. Let's have a look on this paper.

## Contribution

1. Do not resample particles
2. Do not rely on proposal density
3. Compute the log of the unnomalized density using particle flow
4. Do not use importance sampling or any MCMC

## Derivation of PDE
Compute a flow of particles induced by the following flow of the conditional unnormalized probability density of x:
$$
\log p(x,\lambda)=\log g(x)+\lambda logh(x)
$$
x is the d-dimensional state vector, g(x) denotes the prior unnormalized probability density of x, h(x) is likelihood. In other words, we can regard h(x) as measurement likelihood in our AV tracking system, such as certainty of vision or auditory detection (darknet, mask-rcnn and etc.). x is more similar to system  description, we will use this to represent how will algorithm to assign particles. Variable $\lambda$



