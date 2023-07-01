---
layout: post
title: Fairness in MOT
categories: Computer Vision
tags: 
comments: true
---

(From [Zhang et al, 2020](https://arxiv.org/pdf/2004.01888.pdf))
>"We learn re-ID features through a classification task. All object instances of the same identity in the training set are treated as the same class. For each GT box in the image,  we  obtain  the  object  center  on  the  heatmap. We extract the re-ID feature vector and learn to map it to a class distribution vector."


Zhang et al propose a joint strategy to detect objects and also address the re-id task. They state that many existing works address this task by two separate models - one, detection model which localises the objects of interest by bounding boxes and two, an asssociation model which extracts re-identification features for each bounding box and links it to one of the tracks according to metrics defined on the features. 

The authors claim that this approach is not useful for real-time tracking (this is currently not the primary requirement in the tracking of biological objects!). They cite the work of [Voigtlaender et al](https://openaccess.thecvf.com/content_CVPR_2019/papers/Voigtlaender_MOTS_Multi-Object_Tracking_and_Segmentation_CVPR_2019_paper.pdf) who added a re-id branch onto mask rcnn. The authors claim that this joint approach actually lead to a drop in tracking accuracy and use this to motivate that combining the two tasks is non-trivial and should be addressed with care. 

The authors call their work `FairMOT`: here they attempt to address three `fairness` issues prevalent in previous one-shot tracking approaches. One, even with previous methods such as TrackR-CNN and JDE that perform joint joint detection and tracking, they feel this is done in a cascaded style (detection first, re-ID second) which biases the re-identification task on the quality of the proposal. Two, they provide an intuition that object detection requires features from several layers, while re-ID task requires only low-level features (which I thought was an interesting insight!). Three, they empirically state that learning low dimension features for the re-ID task is more beneficial than a high-dimensional feature as the latter has a detrimental impact on the final detection accuracy which in turn affects the tracking accuracy.

One interesting insight for me was that they address the re-id task as a classification problem. All instances of the same object in the training data are treated as the same class, the re-id feature is mapped to a class distribution vector, which allows  evaluating multi-class entropy. How many classes should there be in MOT-17, I wonder? Interesting that it works even for a dataset with 1000+classes. 
