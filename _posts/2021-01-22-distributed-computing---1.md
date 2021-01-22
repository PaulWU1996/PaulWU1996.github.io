---
layout: post
title: "Distributed Computing   1"
subtitle: "Overview of Distributed Computing and Basics"
date: 2021-01-22
author: "Peipei WU"
header-img: 
header-style: text
header-mask: 
tags: [distributed computing]
mathjax: true
---

# Distributed Computing Basics

Hello everyone! Today, I will create a new series about distributed computing! I will introduce this concept based on book  [^1] . We will focus on first 8 chapters at first. In serires, I will add some contents to analysis relationship between distributed computing and AV tracking tasks. After that, we will try to implement this method on our AV tracking tasks.

## Overview of this Series

    1. Introduction
    2. Architectures
    3. Processes
    4. Communication
    5. Naming
    6. Coordination
    7. Consistency and replication 
    8. Fault tolerance

Today, we are going to discuss the first point, introduction. This chapter covers definition and goals.

## Definition 

First, we need to determine the definition of distributed computing.

___A distributed system is a collection of independent computers that appears to its users as a single coherent system.___

It means that one distributed system consits several components, they are autonomous. No matter they work in same time or not, the whole system works as single one. And compare to parallel system, each component in distributed system owns its prviate memory. It is different to parallel system that no global clock is required.

## Goals

+ **Making resource accessible:** the main goal of distributed system is to make users access remote resource easier in controlled and efficeient way.  This is good for AV tracking application, especially when compute resource is not enough, part of algorithm and its previous result can be transferred to resource rich nodes.

+ **Distribution transparency:** hide the fact that its processes and resources are physically distributed across multiple nodes. It is easier to understand that we feel the whole system works smoothly, all components works together as a combination.

  | transparency | Description                                                  |
  | ------------ | ------------------------------------------------------------ |
  | Access       | Hide difference in data representation and how resource is accessed |
  | Location     | Hide where the resource is located                           |
  | Migration    | Hide any possible movement of resource (move to other nodes) |
  | Relocation   | Hide any possible movemennt of resource during it is being used |
  | Replication  | Hide it is replicated                                        |
  | Concurrency  | Hide resource may be shared by several competitive users     |
  | Failure      | Hide failure and recovery of resource                        |

+ __Openness:__ In own words, it requires the whole system offer service according to standard rule, no matter difference existed between nodes but they can be connected by standard rule.

+ __Scalability__: Scale of distributed system should be dynamic, add or delete nodes are freely.

+ __Pitfalls:__ What we need to avoid and we should obey the rules following :

      1. network is reliable
         2. network is secure
         3. network is homogeneous
         4. topology does not change
         5. Latency is zero
         6. Bandwidth is infinite
         7. transport cost is zero
         8. one admin

## Endnote

We just talked come basic concepts about distributed system today. Our goal is to implement one distributed AV tracking system with robust and in real time. We clearify the definition and give the standard rules for developing in the future. So sorry that I did not complete the comment API that hope readers can reply via email to [email](p.wu@surrey.ac.uk) .I gave up the API offered by Hux thus I need to write for myself, also thanks to him again!

## Reference

[^1]:   Tanenbaum, Andrew S., and Maarten van Steen. Distributed Systems : Principles and Paradigms . Second edition, adjusted for digital publishing., [Createspace Independent Publishing Platform], 2017.

