---
layout: post
title: Geometric Feature Matching  
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Zhang and Lee, 2019](https://openaccess.thecvf.com/content_ICCV_2019/html/Zhang_Deep_Graphical_Feature_Learning_for_the_Feature_Matching_Problem_ICCV_2019_paper.html))
>"The main drawbacks of vanilla MPNN is the memory consumption. Assume that we are working on a graph with 100 nodes, each node is associated with 10 edges, and din=dout = 512, then we will need approximately 100×10×5122×4≈1GB memory for storing all kij in single float precision. Doing back-propagation with kij may require an additional 3-4GB memory. As a result, several MPNN layers can easily exhaust the GPU memory. For large scale data, a more efficient version of MPNN is required."


Zhang and Lee state that traditional methods extract rich unary-based local features (image intensities, higher order intensity derivatives etc) through which they can achieve robust feature matching. But if the feature were simply the position of the node, then pairwise or higher order geometric features need to be extracted, which causes the matching of the nodes to become an NP hard problem. Many works have addressed this problem by trying to achieve relaxations of this Np-hard problem. Zhang and Lee follow a different route - they claim that the point coordinates for the vertices of two graphs can be transformed into rich local geometric features using a graph neural network, and these can be subsequently matched through the Hungarian Algorithm. I suppose this would be equivalent to say letting the network design a feature that encapsulates the description of the local neighbourhood, which traditional methods such as Belongie et al handcrafted implicitly?


