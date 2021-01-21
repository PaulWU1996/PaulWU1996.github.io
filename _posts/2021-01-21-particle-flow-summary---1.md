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
$$logp(x,\lamda)$$  
