---
layout: post
title: Point Track
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Xu et al, 2020](https://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123460256.pdf))
>"As affected by the inherent receptive field, convolution based feature extraction inevitably mixes up the foreground features and the background features resulting in ambiguities in the subsequent instance association. In this paper, we propose a highly effective method for learning instance embeddings based on segments by converting the compact image representation to unordered 2D point cloud representation"

Xu et al propose a new approach for multi object tracking and segmentation. Their method and intuition draws from the idea that for associating instances temporally, one uses learnt convolutional features on instances. But this has the disadvantage of mixing up the background and foreground features. Hence, instead they propose  a two stage process where in the first stage, a segmentation network learns to perform instance segmentation on individual frames and in the second stage, another network learns deep features for randomly sampled pixels from the foreground which further aid the tracking process. Additionally, the authors propose a new dataset called APOLLO which includes 68 % more dense images (as compared to the KITTIMOTS dataset).

The authors consider four data modules (offset, color, category and positional embedding) as a bank of features on pixels (points) arising from the foreground or background - then, using a pointnet-like network, these features are transformed to become rich contextual features - this allows associations between frames to happen in a more robust manner during the inference stage. 

As far as I could deduce [currently](https://github.com/detectRecog/PointTrack/blob/520ba222e4de7ca15a127cd3cc1b5c6cb35ca112/models/BranchedERFNet.py#L206), this point-net like network is trained through triplet loss where embeddings of pixels arising from an object instance at multiple time points are pushed together and away from embeddings of pixels arising from another object instance - in this manner, the authors incorporate GT tracking data. 






