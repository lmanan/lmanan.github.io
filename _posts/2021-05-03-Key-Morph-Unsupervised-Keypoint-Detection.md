---
layout: post
title: Unsupervised Keypoint Detection  
categories: Computer Vision
tags:
mathjax: true
comments: true
---


(From [Ma et al, 2020](https://arxiv.org/pdf/2003.01639.pdf))
>"Our building block is a shift-equivariant network that combines a U-Net and a center-of-mass layer to produce predicted landmark co- ordinates in a volumetric image of a computationally manage- able size. Blocks operating at different scales are then con- nected through differentiable resampling layers, which allows us to train the whole network end-to-end. Unlike heatmap based approaches, the proposed model directly computes spatial coordinates for the landmarks. "

Ma et al propose a new architecture for detecting keypoints. Normally this is done by representing the ground truth as a gaussian centered on the keypoints with an arbitrary standard deviation. The task then is to predict this gaussian mask which is somewhat difficult because most of the signal is composed of zeros and there is only a small region with non zero values. During inference, the local maxima of the predicted gaussian mask is used to localise the keypoints.

The authors instead directly predicting the coordinates of the keypoint during training. This is done by having a center of mass layer which weighs the x, y and z coordinates. The weights are the value of the image predicted in the penultimate layer. In my understanding, this means that:

$$
x_{kp} = \frac{1}{J \times K} \frac{\sum^{H}_{i=1} I_{ijk} i}{\sum^{H}_{i=1} i}
$$
