---
layout: post
title: Differentiable Mutual Information
categories: Computer Vision
tags:
mathjax: true
comments: true
---
(From [Guo](https://dspace.mit.edu/handle/1721.1/123142), 2018)
>"Equation (2.2) is equal to the Kullback-Leiber divergence between the probability distributions $p(a, b)$ and $p(a)p(b)$. Therefore, $I(A, B)$ is maximized when the two distributions are the least similar. Since $p(a)p(b)$ represents the distribution of pixel intensities if the two images were independent, $p(a, b)$ and $p(a)p(b)$ should be the least similar when the two images A and B are aligned, because it maximizes the amount of information one image gives about the other.
Mutual information is a loss function that has been shown to work with multi-modal image registration. To use mutual information with Voxel-Morph, it must be approximated in a differentiable way. Intuitively, each voxel should contribute continuously to a range of histogram bins, instead of contributing only to the bin it falls into."

