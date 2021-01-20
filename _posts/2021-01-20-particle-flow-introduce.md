---
layout: post
title: "Particle Flow Introduction"
subtitle: "Summary of PFlow history"
date: 2021-01-20
author: "Peipei WU"
header-img: 
header-style: text
header-mask: 
tags: [particle flow ]
mathjax: true

---
# Particle Flow given by Daum
Hello, I'm going to take a walk on history of particle flow development. Also thanks to Dr.[Liu Yang]<https://sanglongbest.github.io>, he introduced particle flow to me and presented his excellent work in past. We are studied under same professor [Wang Wenwu]<https://surrey.ac.uk/people/wenwu-wang> in CVSSP at University of Surrey. In this note, we do only summary main thinkings and contributions in development history, more details of each different particle fiow algorithm will be discussed in future. Of course, as a fresher, I cannot include everything in this note. If you find any mistakes or any suggestions, I hope you can point out and tell me please.

## Why we need particle flow
In past, particle filter is one good solution in tracking task. I hope to use this method as one baseline in my AV tracking task. The fact that makes us to select particle flow not traditional particle filter is not good enough. There is no doubt that particle filter can mitigate the curse of dimensionality for certain filtering problems, however, it cannot avoid curse of dimensionality in general. Especially when situation becomes more complex, the number of dimension increases to big number, traditional filter cannot work well. To handle this problem, particle flow was introuduced.

## The history of particle flow
1. Ancient time
    *[Nonlinear filters with log-homotopy] In this paper, particle flow is first proposed. Due to peaks of likelihood and prior densities are different, particles are hard to collect high likelihood and possibility at  the same time. Based on Bayes equation, the posterior cannot be represented accurately. Author introduces log-homotopy to estimate the processing from priot density to posterior densty.
    *[Nonlinear filters with particle flow] In this paper, an exact solution for particle flow corresponding to Bayes' rule is first given based on the prior density and the likelihood are Gausian densities. 
    *[Exact particle flow for nonlinear filters] In this paper, proposal density is given up for it is hard to get from real world, also it must be provided in other particle filters. No need to use resanple on a target. Compute Bayes' rule by paricle flow not traditional way. Noted that particle flow corresponds to the exact flow of CPD (conditional probability density) and it can be used in general situation.
2. 2010s
    *[Particle flow for nonlinear filters] Many thanks to Dr. Liu Yang for his suggestion. This paper is great for freshers like me, it includes all information performed by Daum before 2011. The paper and slide are offered in the [link]<https://www.superlectures.com/icassp2011/lecture.php?lang=en&id=351>. I will push one note to talk about it in details on weekends, I do hope I can publish it out! LoL!
    *[Discussion and Applicatin of the Homotopy Filter] Another thinking to understand particle filter, but I did not read it at all. So sorry about that, but I will publish one note in future to discuss about it if possible.
    *[Coulomb's law particle flow for nonlinear filters] In this work, they give up importance sampling or any MCMC, also they run faster than standard particle filters under same accuray.
    *[Particle flow with non-zero diffusion for nonlinear filters] It first proposed non-zero diffusion. It assumes the diffustion part is non-zero, also much more simple and faster than zero diffusion particle flow.
    *[Stochastic Particle Flow for Nonlinear High-Dimensional Filtering Problems] Renormalization grou flow in k-space for nonlinear filters, Bayesian decisions and transport. All equations are given, thus we will discuss it in future with a complete note.
    *[New theory and numerical experiments for Gromov's method with stochastic particle flow] Gromov's method is used for non-Gaussian nonlinear problems, which is suitable to problems in real world. The covariance matrix of diffusion is simplfied.
3. Video
    *[link1]<https://www.pathlms.com/siam/courses/7376/sections/10637/video_presentations/102584> [link2]<https://www.youtube.com/watch?v=vqJGB47XoeY&t=7s> These two videos are very useful, unfortunatelly Chinese users (PRC) cannot use them due to GFW. If you have VPN, do hope you can enjoy!
## Endnote
This is my first note in my blog. I do hope all of you can enjoy it. I will keep publishing new notes and new records here! 
    
