---
layout: post
title: Spectral Matching
categories: Computer Vision
tags:
mathjax: true
comments: true
---
(From [Leordeanu et al](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=1544893), 2009)
>"Our method is based on the observation that the graph associated with M contains: 1. a main strongly connected cluster formed by the correct assignments that tend to establish agreement links (edges with positive weights) among each other. These agreement links are formed when pairs of assignments agree at the level of pairwise relationships (e.g. geometry) between the features they are putting in correspondence 2. a lot of incorrect assignments outside of that cluster or weakly connected to it, which do not form strongly connected clusters due to their small probability of establishing agreement links and random, unstructured way in which they form these links."

In the [publication](https://www.ri.cmu.edu/pub_files/2009/6/2066.pdf) by Leordeanu and colleagues, the authors show how to perform parameter learning for the purpose of graph matching in an unsupervised fashion. According to the authors, there are only two publications (one of which is the work by Caetano et al) which propose a solution for learning the optimal set of parameters. They reason that unsupervised learning for graph matching is important because manual annotation of correspondences is time-consuming. But additionally if supervised data is available, such data can be used.

The graph matching problem arises from 
$$x^{*}= \text{arg max} \left( x^{T} M x \right) $$ where $$x_{ia} = 0 \text{ or} 1$$. An important thing to note is that M is a matrix with positive elements such that $$M_{ia; jb}$$ measures how well the features (i,j) from one image agree in appearance and geometry with features (a, b) from image two. These matrices can be quite large, for example, for comparing two images with 300 fully connected graph nodes, M has size 90000 X 90000 ~ 8000 M parameters. In such problems, typically the formulation of the unary and pair-wise features is known, but not their relative importance or weights $$w$$. Then learning for graph matching consist of finding $$w$$ that maximises performance (with respect to ground truth correspondences) of matching over pairs of training images. It appears to me that the number of learnable parameters are quite small, in this formulation. Did Caetano et al also have so few learnable parameters?

The mentioned algorithm leverages the statistical properties of the matrix $$M$$ and its leading eigen vector $$v$$ which is used to find a binary solution. Let $$ p_{1} > 0 $$ be the expected value of the correct, second-order scores between correct asignments. And let $$ p_{0} > 0 $$ be the expected value of the second-order scores between incorrect assignments (atleast one of the corresponences is assumed wrong). It is reasonable to conclude that $$p_{1} > p_{0}$$ since the pairs of correct assignments should agree in appearance and geometry. The ratio $$p_{r} =\frac{p_{0}}{p_{1}}$$ can therefore comment on the quality of the matching : if $$p_{r}$$ is low, then the matching is good and vice-versa. 
 


