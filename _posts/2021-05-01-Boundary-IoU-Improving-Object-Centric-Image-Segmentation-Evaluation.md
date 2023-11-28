---
layout: post
title: Boundary IoU: Improving Object-Centric Image Segmentation Evaluation 
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Cheng et al, 2021](https://arxiv.org/abs/2103.16562))
>"The common task framework in which standardized tasks, datasets and evaluation metrics are used to track research progress yields impressive results. For example, researchers working on the instance segmentation task, have improved the standard Average Precision (AP) metrics on COCO by an astonishing 86 % from 2015 to 2019".

Cheng et al, 2021 state that a majority of Mask RCNN-based methods predict low fidelity, blobby masks. This leads them to conclude that despite these methods getting better, the current evaluation metrics (Average Precision etc) may have limited sensitivity to mask prediction errors near object boundaries.

To understand why, the authors say the following:

>"Mask IoU divides the intersection area of two masks by the area of their union. This measures values all pixels equally and therefore is less sensitive to boundary quality in larger objects: the number of interior pixels grows quadratically in object size and can far exceed the number of boundary pixels, which only grows linearly."

According to the authors, the number of interior pixels grows quadratically while the number of boundary pixels grows linearly - which makes sense since the number of interior pixels, for a perfect circle, would be ~$\pi r^{2}$ while the number of boundary pixels would be $2 \pi r$. Thus on larger objects, for better performance on the current evaluation metrics, it is necessary to care about interior pixels more than boundary pixels.


The authors highlight typical errors in this nice figure:

<p><figure><img src="/images/2021-05-01/segmentation-errors.png" alt=""/></figure></p>

