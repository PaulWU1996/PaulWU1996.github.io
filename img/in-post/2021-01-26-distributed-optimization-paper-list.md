---
layout: post
title: "distributed optimization paper list"
subtitle: "paper list summary"
date: 2021-01-26
author: "Peipei WU"
header-img: 
header-style: text
header-mask: 
tags: [distributed optimization]
mathjax: true
typora-root-url: paulwu1996.github.io
---

# Distributed Optimization Paper List

Recent distributed optimization algorithms can be divided into 2 different categories based on the data type, discrete or continuous. I summary the paper list based on survey given by Tao. Basic knowledges are introduced in previous note. You can have check with graph theory, convex analysis and convergence analysis details. This note I will focus on some basic algorithms.

## Discrete 

### Algortihns with diminishing step-sizes

| Approach                                                     | Year | Contribution                                                 | Limitation                                     |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ---------------------------------------------- |
| distributed (sub)gradient descent                            | 2009 | Two steps of this algortihm, first is consesus step and then descent step along the local (sub)gradient direction of each agent convex function. | it requires fixed undirected graph; it is slow |
| distributed augmented Lagrangian with multiple consensus step in the inner loop | 2014 | developed a fast distributed algorithm based on the centralized Nesterov gradient method, if local objective functions are smooth. | Slow                                           |



### Algorithms with a fixed step-size

| Approach                                                | Year | Contribution                                                 | Limitation                         |
| ------------------------------------------------------- | ---- | ------------------------------------------------------------ | ---------------------------------- |
| EXTRA                                                   | 2015 | Add one correction term in DGD, this helps that even step-size is fixed, the final optimal solution could be accurately. |                                    |
| PG-EXTRA                                                | 2015 | developed a proximal gradient exact first-order algorithm based on proximal operations(2014 Parikh) |                                    |
| Distributed Forward-Backward Bregman Splitting (D-FBBS) | 2018 |                                                              |                                    |
|                                                         |      |                                                              |                                    |
| DIGing                                                  | 2017 | add one state to describe the average gradient at time k     | identical step-size for all agents |
|                                                         |      |                                                              |                                    |
| Aug-DGM                                                 | 2015 | CTA diffenret step-size to agent                             |                                    |
| AsynDGM                                                 | 2018 | ATC faster                                                   |                                    |
| ATC-DIGing                                              | 2017 | ATC                                                          | 0                                  |



| Approach       | Year | Contribution                                                 | Limitation |
| -------------- | ---- | ------------------------------------------------------------ | ---------- |
| Distributed PI | 2016 | To correct the error caused by the distributed gradients with gain param introduced |            |



## Continuous-time Algorithm

This is more close to our distributed sensor networks.



