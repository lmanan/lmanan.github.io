---
layout: post
title: Tracking by Weakly Supervised Learning and Graph Optimisation 
categories: Computer Vision
tags:
mathjax: true
comments: true
---


(From [Hirsch et al, 2022](ihttps://openreview.net/forum?id=icUFpH3Dq6e))
>"Linajea implements a 4d U-Net to predict the position and movement of each nucleus. Position is encoded as a single channel image of Gaussian shaped blobs, one per nucleus. The locations of the respective intensity maxima correspond to nuclei center points. Movement is encoded as 3d vectors per pixel within a nucleus. Each vector points to the spatial location of the same nucleus in the previous time frame. The four output channels necessary for the above encoding are trained jointly via L2 loss."

Hirsch et al build upon Linajea, where a 4d convolutional network was used to learn position and movement per nucleus. I found it interesting that this was treated as a 4d problem and not a 3d+channel problem - for example, naively one could consider the volumes across the time dimension to be concatenated as channels. The disadvantage of doing this would be that since the contribution from various channels would be merged, one wouldn't be able to pinpoint the location of each cell if it has moved between time points. 

>"Linajea's objective is a weighted sum of the costs of the selected nodes and edges. There are four tunable weights. A constant cost for selecting a node, a factor to scale the position prediction, a factor to scale the distance between predicted movement vector target abd linked cell position, and a constant cost for each track."

The authors make two contributions: they classify cells to be able to predict the cell state, and they use  structured SVM to automatically find optimal weights for the ILP objective.

I wonder if they enforce any sequence information on the cell state - since Prophase is always followed by metaphase, then anaphase and then telophase and this continues cyclically. This hard constraint on the sequence could be used to post process the cell state predictions from the model. 

The authors allow prediction as one of four classes - a parent cell, a daughter cell that has just divided, a cell that is just continuing (between cell divisions) and a polar body. For this task, they train a 3d resnet.   


