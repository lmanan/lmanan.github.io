---
layout: post
title: Recurrent All-Pairs Field Transforms for optical Flow
categories: Computer Vision
tags:
mathjax: true
comments: true
---


(From [Teed and Deng, 2020](https://arxiv.org/pdf/2003.12039.pdf))
>"RAFT  consists  of  three  main  components:  (1)  a  feature  encoder  that  extracts a feature vector for each pixel; (2) a correlation layer that produces a 4D correlation  volume  for  all  pairs  of  pixels,  with  subsequent  pooling  to  produce lower resolution volumes; (3) a recurrent GRU-based update  operator that retrieves values from the correlation volumes and iteratively updates a flow field initialized at zero."


[Teed and Deng, 2020](https://arxiv.org/pdf/2003.12039.pdf) provide a novel and elegant framework for estimating optical flow by training a network in a supervised fashion.  Similar to Neighbourhood Consensus Networks from [Rocco et al, 2018](https://www.di.ens.fr/willow/research/ncnet/), the authors estimate a 4D correlation matrix by comparing the per pixel features of image one to per pixel features of image two. These are then pooled in the last two dimensions at varying resolution levels, in order to construct a correlation pyramid which describes how similar the feature obtained from each pixel of the original image 1 is to features obtained from each pixel of a downsampled version of image two.   
