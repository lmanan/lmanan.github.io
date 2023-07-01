---
layout: post
title: Graph Mixup with Soft Alignments
categories: Computer Science
tags:
mathjax: true
comments: true
---

(From [Zhang et al, 2018](https://arxiv.org/pdf/1710.09412.pdf%C2%A0)
>"In most successful applications, thee neural networks share two commonalities. First, they are trained to minimize their average error over the training data, a learning rule also known as the Empirical Risk Minimization principle. Second, the size of the state of the art neural networks scales linearly with the number of training examples."

Zhang et al state that empirical risk minimization allows neural networks to memorize (instead of generalize) from the training data. Also, it causes neural networks trained with ERM to change their predictions when tested on examples outside the training distribution. In contrast, vicinal risk minimization allows one to expand the training distribution by drawing virtual samples from the vicinity of the actual training examples. The authors say that while performing image classification, it is common to define the vicinity of an image as the set of its horizontal reflections, but state that the choice of augmentations is often dataset dependent. Motivated by these ideas, the authors proposed a data agnostic augmentation technique called mixup.  

[Ling et al, 2023](https://openreview.net/forum?id=zS6QCVwPUs) state that while data augmentations are relatively straight forward for grid-like images, they are particularly challenging for grid-like data. 

>"A key difficulty lies in the lack of simple graph operations that preserve the original labels, such as rotations on images. Most existing graph-based operations assume that the labels are the same after simple operations like dropping a random node or edge, on training graphs. On the one hand, such simple operations may not be able to generate diverse new samples. On the other hand, although the operations are simple, they are not guaranteed to preserve the original labels."

The authors cite mixup work done previously on images and state that would be an effective strategy to apply on graphs. But while finding a convex combination of images is possible, how does one apply mixup on graphs, when typically graphs have different number of nodes. Even for graphs with the same number of nodes, the node correspondence is not available.

>"Can we conduct image-like mixup for graphs with node-level correspondence to preserve critical information?"

