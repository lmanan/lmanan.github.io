---
layout: post
title: Point Nets
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Qi et al](https://arxiv.org/pdf/1612.00593.pdf), 2016)
>"Our input is a subset of points from an Euclidean space. It has three main properties: (1) Unordered: Unlike  pixel  arrays  in  images  or  voxel arrays in volumetric grids, point cloud is a set of points without specific order. In other words, a network that consumes N 3D point sets needs to be invariant to N! permutations of the input set in data feeding order. (2) Interaction among points: The points are from a space with a distance  metric. It means  that  points  are  not isolated,  and  neighboring  points  form  a meaningful subset. Therefore,  the  model  needs  to  be  able  to capture  local  structures  from  nearby  points,  and  the combinatorial interactions among local structures. (3) Invariance  under  transformations: As  a  geometric object, the  learned representation  of  the  point  set should  be  invariant  to  certain  transformations.  For example,  rotating  and  translating  points  all  together should not modify the global point cloud category nor the segmentation of the points."


>"In order to make a model invariant to input permutation, three strategies exist: 1) sort input into a canonical order; 2) treat the input as a sequence to train an RNN, but augment the training data by all kinds of permutations; 3) use a simple symmetric function to aggregate the information from each point. Here, a symmetric function takes n vectors as input and  outputs a new vector that is invariant to the input order. For example, + and ∗ operators are symmetric binary functions."

Pointnets show incredibe promise for the task of object classification, but they have not been used so much for pose estimation. Since the network should output different poses for differently transformed state of the three dimensional input point cloud, I wonder if the T-net used in the publication should not be used. Also how many parameters should be estimated for 3d pose estimation - I suppose that the centroid of the object is always known by looking at the centre of mass of the input cloud, then that leaves 6 d.o.f to be estimated?  

Particularly intreresting feature about the pointnet is its robustness to clutter and missing points. I wonder if the network learns the pose for embryos at a later developmental stage if they have been trained on an earlier stage. 

The next step would perhaps include testing the pytorch variant of the published code. [PointNetLK](https://arxiv.org/abs/1903.05711) offers an interesting approach to register point clouds using pointnet, which should be looked into.
