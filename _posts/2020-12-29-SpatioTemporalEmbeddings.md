---
layout: post
title: Spatio-Temporal Embeddings
categories: Computer Vision
tags:
mathjax: true
comments: true
---

[Athar and colleagues](https://arxiv.org/pdf/2003.08429.pdf) extend the work by Neven et al - instead of only spatially embedding pixels as had been done previously for the task of instance segmentation, they attempted to consider time as an additional dimension and achieve joint segmentation and tracking with their method called `STemSeg`.

(From Athar and colleagues)
>"We learn to segment object instances in videos in a bottom-up fashion by leveraging spatio-temporal embeddings. To this end, we propose an efficient, single-stage network that operates directly on a 3D spatio-temporal volume. We train the embeddings in a category-agnostic setting, such that pixels belonging to the same object instance across the  spatio-temporal  volume  are  mapped  to  a  single  cluster  in  the  embedding space."

The contributions of the authors, in my opinion, include extending Neven et al to the task of joint detection and tracking, coming up with a novel architecture which allow 3d (x,y and time) convolutions, introducing the idea of a learnt free dimension, and several experiments across different datasets which provide proof of the success of their approach.

In discussions with a colleague, we stumbled upon the same extension and potential appplication of Neven et al to joint detection and tracking, a few months ago. One question which intrigued me then was how to consider the time dimension vis a vis the spatial dimensions. Originally the pixel space was first converted to the metric space - this allows, for example, to have anisotropic pixel resolutions in ones' images, but a resultant isotropic metric resolution. Then the predicted offset vectors resulting from any pixel location, also live in the metric space. I reasoned that there could be a anisotropy factor which is roughly physically equivalent to the velocity of the pixel and which if applied to the time dimension translates it to the metric space (this seems logical to me since $\text{distance} = \text{velocity} \times \text{time}$). This factor could potentially be learnt by the network.

The authors of `STemSeg` came across a similar quandary but followed a different approach.


