---
layout: post
title: Sparsifying Neural Net Architectures
categories: Computer Vision
tags:
mathjax: true
comments: true
---

The Lottery Ticket Hypothesis publication, which won the ICLR 2018 best paper award provides some clean strategies on how to sparsify one's network. The authors show that pruning a neural network in the manner prescribed would allow reducing parameters by 90 % while obtaining faster convergence times and slightly higher accuracies.

(From [Frankle and Carbin, 2018](https://arxiv.org/pdf/1803.03635.pdf))
>"We demonstrate that pruning uncovers trainable subnetworks that reach test accuracy comparable to the original networks from which they derived in a comparable number of iterations. We show that pruning finds winning tickets that learn faster than the original network while reaching higher test accuracy and generalizing better. We propose the lottery ticket hypothesis as a new perspective on the composition of neural networks to explain these findings"

I thought this was a very intriguing work especially in the context of 3d convolutional neural networks typically used to train on volumetric images in the biomedical image domain. Even setting a batch size of 2 sometimes leads to more than 12 GBs of GPU memory usage, which most often, personal workstations do not support. 
