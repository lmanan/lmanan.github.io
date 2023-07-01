---
layout: post
title: Inter and Intra Clusters
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-05-12/brabandere.png" alt=""/><figcaption>
[Source: Brabandere et al, 2017. The network maps each pixel to a point in feature space so that pixels blonging to the same instance are close to ecah other and can be clustered with a fast post-processing step. From top to bottom, left to right: input image, output of the network (before post processing), pixel embeddings in 2-dimensional feature space, clustered image]</figcaption></figure></p>

(From [Brabandere et al, 2017](https://arxiv.org/pdf/1708.02551.pdf))
>"In our loss function we keep the first term, but replace the  second  term  with  a  more  tractable  one:   instead  of directly  penalizing  small  distances  between  every  pair  of differently-labeled embeddings, we only penalize small distances between the mean embeddings of different labels. If the number of different labels is smaller than the number of inputs, this is computationally much cheaper than calculating the distances between every pair of embeddings. This is a valid assumption for instance segmentation, where there are orders of magnitude fewer instances than pixels in an image."

The authors here came up with a clever strategy: instead of considering all possible inter cluster pixel-pairs, they rather chose to consider only the pair-wise mean embeddings from each cluster, which is a computational less costly task. The authors then came up with a beautiful interpretation of the two loss terms - they say that when the variance and distance loss terms are zero, would imply that each pixel embedding is within a distance of $$\delta_{v}$$ from the cluster centre and that each cluster centre is atleast $$ 2 \times \delta_{d}$$ distance apart from another cluster center. 

The authors made a powerful observation that for the semantic instance segmentation to be accurate, the semantic segmentation has to be precise. I wonder why they did not suggest a combined loss function which includes a semantic loss and the other terms which they suggest (variance term, distance term). Also it is not clear how impotant is the regularization term.
