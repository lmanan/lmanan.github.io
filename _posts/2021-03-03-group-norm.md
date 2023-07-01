---
layout: post
title: Group Norm 
categories: Computer Vision
tags: 
comments: true
---

(From [Wu et al, 2020](https://arxiv.org/pdf/1803.08494.pdf))
>"We notice that many classical features like SIFT and HOG are group-wise features and involve group wise normalization. For example, a HOG vector is the outcome of several spatial cells where each cell is represented by a normalized orientation histogram. Analogously, we propose GN as a layer that divides channels into groups and normalizes features within each group. GN does not employ the batch dimension and its computation is independent of batch sizes."


Wu et al provide an interesting perspective that the channels of visual representation are not entirely independent. They give the examples of SIFT and HOG methods where each group of channels is constructed by a kind of histogram. Hence they extend this idea to DNNs: a minimum working example, they quote, is that it is reasonable to expect that a convolutional filter and its horizontal flip will exhibit similar responses on natural images and hence could be grouped together. They empirically state that other factors like frequency, shape etc could also be used for grouping. Motivated by these ideas, they propose normalizing across a group of spatial channels. 

This was a cool idea and has been around for already some time. It should alleviate the problem of reducing the batch size (when you are constrained by GPU memory) and suffering from non-robust batch statistics while calculating batch norm as a consequence - since the group normalization is completely independent of the batch dimension. A small semantic sidenote, I found out that the correct way of referring to batch norm is spatial batch norm as it is a normalization over the batch and spatial dimensions.  

Using groupnorm in pytorch is easy:
```
import torch.nn as nn
gn = nn.GroupNorm(number_groups, number_channels) # use this layer in a model
```
