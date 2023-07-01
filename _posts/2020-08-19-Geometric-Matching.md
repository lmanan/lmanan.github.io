---
layout: post
title: CNN for Geometric Matching
categories: Computer Vision
tags:
mathjax: true
comments: true
---


[Rocco and colleagues](https://arxiv.org/pdf/1703.05593.pdf) estimate the parameters of an affine transform or a thin plate spline transform by using a CNN network. They do so by proposing an architecture for geometric matching. The architecture is based on three chunks: feature extraction, matching and filtering for inliers and affine parameters estimation.

The authors state that at the time of their publication, the traditional learning approach was to learn powerful feature descriptors for image patches. The descriptors are then compared by outputting a similarity score. In this work, the authors take a different approach by considering eth complete image as a whole, which the authors claim should allow for capturing interaction of different parts of the image to a greater extent. The authors use a pretrained VGG network 



The authors use a siamese architecture by passing two images into a network, which allows extraction of feature layers, next these feature layers are matched into a tentative correspondence map and then the transform parameters are outputted. While this seems straightforward enough, for my application where obtaining precise matches is more important than an excellent alignment, I feel that the loss function should be different to take into account disparity in matches. 

For the feature matching, the author arrange the correlation map as a 3D tensor, which is furthermore normalized to ensure that the values are between 0 and 1. This is done by including a ReLU layer followed by a L2 normalization. My objection here is that this seems to be assymmetric. Any way to make this symmetric?

Lastly based on the correspondences, the authors use a 2 layer CNN to regress the transformation parameters. This is not clear to me exactly: what non-linear mechanism is learnt to obtain the rgression parameters? Why are there learnable parameters since the optimal transform can be easily computed by a set of correspondences. 

The beauty of this publication is that since dense correspondencesare hard to find, the second pair of images were simulated using random affine transforms. Maybe I could do the same on volumetric live embryo data?

 
