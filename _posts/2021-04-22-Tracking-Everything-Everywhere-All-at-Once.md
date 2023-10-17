---
layout: post
title: Tracking Everything, Everywhere All at Once
categories: Computer Vision
tags:
mathjax: true
comments: true
---


[Wang et al, 2023](https://arxiv.org/pdf/2306.05422.pdf) present a new method for estimating dense and long range motion from a video sequence. They argue that previous approaches operate through limited temporal windows and struggled in scenarios of occlusion and to maintain global consistency of estimated motion trajectories. They propose a method called Omnimotion that first represents a video through a quasi 3D canonical volume. In this volume, one can perform pixel wise tracking via bijections between the local and canonical space. 

To deep dive into this method, let us first look at NeRFs introduced by [Mildenhall et al, 2022](https://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123460392.pdf). The authors represent a static scene as a continuous 5D function which outputs a view-dependent radiance (color) and a volume density (differential opacity) for each point $\textbf{x} in R^{3}$ and direction $\theta, \phi$.

