---
layout: post
title: Deep Snake for Real Time Instance Segmentation
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Peng et al, 2020](https://arxiv.org/abs/2001.01629)) 
>"Given an initial contour, the snake algorithm iteratively deforms it to match the object boundary by optimising an energy function with low-level features, such as image intensity or gradient. While many variants have been developed, these methods are prone to local optima as the objective functions are handcrafted and typically non-convex."

We look at the task of a post-processing refinement scheme for obtaining accurate efficient segmentations. 
Typically, this could be addressed as a pixel classification problem where the pixels within a label mask are classified as background or foreground, by training a model on sparse pixel classes. This has the disadvantage of being restricted by the initial label mask i.e. for example, if the initial label mask partially segments an object, the prediction would remain defective and would not be able to fix the partial segmentation.

Another option would be to use pixel regression - where each pixel on the initial contour predict an offset to the actual contour. This is more flexible and can fix some of the errors in the initial contour proposal.
One way of doing this is by using the star convex assumption where rays are sent out from the predicted object center (predicted because during inference we will only have access to the predicted object center) and intersects the GT mask and the initial noisy contour. The points of intersection on the initial noisy contour should predict the offset to the corresponding points of intersection on the actual GT contour. 

The problem here is that if the objects inside are non-star convex then the offsets for extremal points would not be suitably learnt.

An alternative to this is provided by Deep Snake, where $N$ points are sampled on the initial contour and the GT contour. The correspondences are established by assigning the point on the GT contour closest to the top extremal point on the initial contour. Next, the features at these points are concatenated with the actual spatial coordinates and then passed through a graph convolutional network. 

On using CNNs to predict offsets, the authors say the following:

>"An alternative method is to use standard CNNs to regress a pixel-wise vector field from the input image to guide the evolution of the initial contour. We argue that an important advantage of deep snake over the standard CNNs is the object-level structured prediction i.e. the offset prediction at a vertex depends on other vertices of the same contour. Therefore, deep snake will predict a more reasonable offset for a vertex located far away from the object. Standard CNNs may have difficulty in this case, as the regressed vector field may drive this vertex to another object which is closer." 

This above makes sense since the CNNs would not perform a convolution over the neighbors of a node but in a grid fashion and that should be inferior to only using the nodes on a contour. 


