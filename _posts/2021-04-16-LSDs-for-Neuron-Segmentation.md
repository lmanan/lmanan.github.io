---
layout: post
title: LSDs for neuron segmentation
categories: Biology
tags:
mathjax: true
comments: true
---
(From [Sheridan et al, 2022](https://www.nature.com/articles/s41592-022-01711-z))
>"Consider the computational time required by the current state of the art, flood filling network (FFN): assuming linear scalability and the availability of 1000 contemporary GPUs, the processing of a complete mouse brain would take 226 years."

The authors propose an auxiliary task aimed at neuron segmentation. The task involves predicting for each voxel, the distance to the center of mass of the region of intersection arising from a gaussian (of fixed size) placed at that voxel and the label mask that voxel is part of. Voxels which are near the border of an object would have offsets pointing inwards while the voxels near the center of an object would have offsets of zero (or small) magnitude. Thus the network is incentivized to pay close attention to the border pixels maybe more than the interior pixels as the border pixels would contribute mostly to the loss gradients (is my understanding!)

>"Intuitively, the LSD components encourage the neural network to make use of the entire field of view to reach a decision about the presence or absence if a boundary in the centre of the field of view."

The authors comment that the LSD based methods are more robust at the task of boundary prediction than affinity-based methods. Through the auxiliary task, each voxel learns the local interior binary mask surrounding it which would, in principle, enable better segmentation. 

It is not obvious to me currently, which inference scheme is eventually used to extract instance segmentations from the densely predicted local shape descriptors at test-time.  
