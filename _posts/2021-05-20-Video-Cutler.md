---
layout: post
title: VideoCutler
categories: Computer Vision
tags:
mathjax: true
comments: true
---

From [Wang et al, 2023](https://arxiv.org/pdf/2308.14710.pdf)

>"Moreover, unlike most prior approaches, we demonstrate that UnVIS models can be learned without relying on natural videos and optical flow estimates"

How do you train UnVIS?

>"Then we convert a random pair of images in the mini batch into a video with corresponding pseudo mask trajectories using ImageCut2Video."

How does ImageCut2Video work?

>"We utilize VideoMask2Former as our video instance segmentation model which operates by attending to the 3D spatiotemporal features of our synthetic viideos and generating 3D volume predictions of pseudo mask trajectories using shared queries across frames."

How does VideoMask2Former work? Is this used for doing 3d convolutions?
Have a look at Figure 1 which is fairly descriptive.

