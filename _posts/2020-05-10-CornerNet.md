---
layout: post
title: CornerNet
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-05-10/cornernet.png" alt=""/><figcaption>
[Source: Law and Deng, 2019]</figcaption></figure></p>

(From [Law and Deng](https://arxiv.org/pdf/1808.01244.pdf))
>"But  the  use  of  anchor  boxes  has  two  drawbacks. First, we typically need a very large set of anchor boxes, e.g. more than 40k in DSSD and more than 100k in RetinaNet. This is because  the  detector  is  trained  to  classify  whether  each anchor  box  sufficiently  overlaps  with  a  ground  truthbox, and a large number of anchor boxes is needed to ensure sufficient overlap with most ground truth boxes. As  a  result,  only  a  tiny  fraction  of  anchor  boxes  will overlap with ground truth; this creates a huge imbalance between positive and negative anchor boxes and slows down training. Second,  the  use  of  anchor  boxes  introduces  many hyperparameters and design choices. These include how many boxes, what sizes, and what aspect ratios. Such choices  have  largely  been  made  via  ad-hoc  heuristics, and can become even more complicated when combinedwith  multiscale  architectures  where  a  single  network makes separate predictions at multiple resolutions, with each scale using different features and its own set of anchor boxes."

In the publication `CornerNet`, the authors take a pose-estimation approach to detecting objects. They preface this implementation with a couple of shortcomings of traditional approaches leveraging anchor-boxes. For one, many anchorboxes are needed to ensure a sufficient overlap with all ground truth boxes present in an image and this leads to an imbalance between positive and negative anchor-boxes and leads to slow-training. Moreover, hyperparameters such as an anchorbox size and aspect ratio needs to pre-defined. So as an alternate proposal, the authors do away with the use of anchor boxes altogether, by letting the network predict the location of the top-left and bottom-right corners for each object. The authors claim that this is actually an easier task than predicting the centre of the bounding box since to localize the correct centre, all four edges need to be localized, but to localize a corner, only two edges need to correctly predicted. This problem is made easier by a `Corner Pooling`, a new type of pooling layer that aims to solve the problem that at times, the corner of a bounding box may lie outside the object. In such cases, a corner can not be localized based on local evidence. Instead, to determine whether there is a top-left corner at a pixel location, one needs to look to the right for the top-most boundary and to the bottom for the left-most boundary. 
