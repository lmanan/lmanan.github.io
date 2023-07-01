---
layout: post
title: Probabilistic Points
categories: Computer Vision
tags: 
comments: true
---
(From [Myronenko and Song, 2009](https://arxiv.org/pdf/0905.2635.pdf))
> "The true underlying non-rigid transformaton model is often unknown and challenging to model. Simplistic approximations of the true non-rigid transformation, including piece-wise affine and polynomial models, are often inadequate for correct alignment and can produce erroneous correspondences. Due to the usually large number of transformation parameters, the non-rigid point sets tend to be sensitive to noise and outliers and are likely to converge into local minima."

Coherent Point Drift (CPD) attempts to model the moving point set as a gaussian mixture model (GMM). It essentially solves a probabilty density estimation problem where, at the optimum, the two point sets become aligned and the correspondence is obtained using the maximum of the GMM posterior probability for a given data point. The inherent assumption behind the transformation is that the GMM centroids move coherently as a group to preserve the topological structure of the point sets.


This raises a couple of questions in my mind: In addition to the transformation parameters, the variances of the gaussian are parameters which would need to be estimated in the existing formulation of CPD. However, I wonder that, say if the points have been extracted by using the Laplacian of Gaussian Scale Space feature, could the already available variance information be utilised (instead of being estimated)?!

Additionally, the fixed point set is modeled as points in the current implementation of CPD. Would it benefit if they were modeled as a GMM as well? Is it correct to say then that the problem would instead become one of minimising information loss between two probability density fields?! 

Lastly, while the assumption of motion coherence makes a lot of sense, I wonder if better results could be obtained by using a model which is based on the Navier Stokes Equation. In that scenario, the fluid parameters would define the transformation and would need to be estimated. In essence, we would then convert the registration problem to one of stress-strain. Since, the Navier-Stokes equation would determine velocties of point features as a function of time, each time step could be considered equivalent to a sub-iteration. Would such a framework be equivalent to the annealing sub-step in [Chui and Rangarajan, 2003](https://www.sciencedirect.com/science/article/pii/S1077314203000092) 's work? 

