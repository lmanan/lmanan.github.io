---
layout: post
title: Learning Voxel Embeddings for Neurons
categories: Computer Vision
tags:
mathjax: true
comments: true
---


[Lee et al, 2021](https://arxiv.org/pdf/1909.09872.pdf) state that the method which simply detected boundaries for the SNEMI3D and CREMI challenge still is the leader after a span of 3 years. They propose that dense voxel embeddings can outperform boundary detection nets on these two datasets.

They state that the general idea of predicting embeddings closer if pixels belong to same object and further away if pixels belong to different objects, is well known but what is unclear is if this extends well to the case when several neurons are intertwined. 

From the learnt embeddings, the authors compute an affinity map. This affinity map is obtained for the whole image by stitching and blending together affinities from overlapping patches. Then by running the mutex watershed algorithm (which employs short range and long range affinities) and agglomerating segmentations with very similar mean embeddings, leads them to beat the boundary detection nets on the dataset leaderboards.

They explain the rationale behind using affinities instead of directly using the embeddings themselves. Essentially using affinities makes it more robust in regions of overlapping image patches where pixel embeddings may be quite opposite.


