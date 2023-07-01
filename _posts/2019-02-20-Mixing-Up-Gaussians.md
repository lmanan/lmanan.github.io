---
layout: post
title: Mixing Up Gaussians
categories: Computer Vision
tags: 
comments: true
---
(From [Jian and Vemuri, 2005](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=1544863)) 
> "Recent work on point matching using Gaussian mixture models has been proposed by [Chui and Rangarajan](https://www.semanticscholar.org/paper/A-new-point-matching-algorithm-for-non-rigid-Chui-Rangarajan/028cd399a9741e6a54a0225d007d323ec24aa1c5); they choose one sparsely distributed point-set as the template density modeled by a Gaussian mixture and treat another relatively dense point-set as sample data. Then the point matching is re-interpreted as a mixture density estimation problem and solved in an EM-like fashion. Instead of the asymmetric point matching case in Chui and Rangarajan, we treat the problem using mixtures in a symmetrical manner. In this way, the two point-sets, model and scene, are represented by two mixtures of Gaussians. Intuitively, if these two point sets are aligned properly enough, the two resulting mixtures should be statistically similar to each other. Consequently, this raises the key problem: How to measure the similarity/closeness between two Gaussian mixtures?"
 
Jian and Vemuri's work goes along the lines of minimizing the distance between two point sets modeled as a mixture of Gaussians. As they point out in the paper, finding a metric which quantifies *correlation* between each pair of points and has a closed form expression, is the key. They suggest using the L2 distance (there is no closed form expression to measure Kullback Leiber divergence between two Gaussian mixtures. But why is that - I don't understand!)

Additionally, [Myronenko and Song, 2009](https://arxiv.org/pdf/0905.2635.pdf) critique the approach of Jian and Vemuri. They say
> "These methods all use explicit TPS parametrization, which is equivalent to a regularization of second order derivatives of the transformation. The TPS parameterization does not exist when the dimension of points is higher than three, which limits the applicability of these methods."

Is it really true that TPS can not be extended to n dimensions? According to the Geometric Tools paper by [David Eberly](https://www.geometrictools.com/Documentation/ThinPlateSplines.pdf), it is possible to do so.  

