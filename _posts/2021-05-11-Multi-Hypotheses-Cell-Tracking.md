---
layout: post
title: Multi-Hypotheses Cell Tracking
categories: Computer Vision
tags:
mathjax: true
comments: true
---


(From [Bragantini et al, 2023]())
>"The proposed method computes cell tracks and segments using a hierarchy of segmentation hypotheses and selects disjoint segments by maximising the overlap between adjacent frames."

If there are two available hypotheses, and one of them predicts larger structures across time while the other hypotheses predicts smaller structures across time, then the average overlap within segmentations predicted from one hypothesis should be similar, no?

>"The segmentation instances should be computed jointly with tracking to avoid dependence on the accuracy of the segmentation method and to leverage both spatial and temporal information."

How do they predict segmentations jointly alongside tracking? My understanding was that they have hypotheses available from an ensemble of methods and they wish to choose from within them. That is not equivalent to saying that segmentation and tracking are predicted jointly. (Or is it?)

>"Our method was developed for fluorescence microscopy images, where cells are engineered to express nuclei-localized fluorescence labels making them easily identifiable from the background."

Is there something in the method where it does not work with phase contrast time lapse and other modalities?

>"Note that due to the additive nature of MTIoU objective, it can lead to over segmentation?"

Intriguing! Is there a way of normalizing with respect to the number of segmentations, so that the cost of the objective function is invariant to number of segmentations.   



