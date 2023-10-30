---
layout: post
title: Learning canonical deformation coordinate space
categories: Computer Vision
tags:
mathjax: true
comments: true
---

From [Lei and Daniilidis, 2022](https://arxiv.org/abs/2203.16529) 
>"If we assume that the topology does not change deformation, all deformed surfaces of one instance can be regarded as equivalent through continuous bijective mappings (homeomorphisms). This allows us to factorize the deformation between two frames by the composition of two continuous invertible functions such that one maps the source frame into a common 3D canonical deformation coordinate space (CaDeX) while another maps it back to the destination frame. Such a factorisation and its implementation is novel, simple and efficient while it guarantees cycle consistency, topology preservation and volume conservation."

The authors say that the desired dynamic representation needs to capture the reference shape and the consistent deformation between any pair. Typically, this is addressed by using any one of the frames as the canonical frame but the authors critique this approach by saying that it complicates the shape prior. Another approach is to calculate an approximate mean, but that can limit the shape expressibility. 

They propose a generic modeling framework. Specifically, it is described as follows. Consider a continuous, bijective mapping (homeomorphism) $H_{i}: R^{3} - R^{3}$ at time $t_{i}$ that maps each deformed coordinate to its global 3d coordinate.
Note that this global 3d coordinate has no time index and can be seen as a global consistent indicator of each correspondence trajectory across time. This $uvw$ space is referred to as CaDeX by the authors.

$$
[x^{j}, y^{j}, z^{j}] = F_{ij}([x^{i}, y^{i}, z^{i}]) = H^{-1}_{j} \circ H_{i}([x^{i}, y^{i}, z^{i}]).
$$






