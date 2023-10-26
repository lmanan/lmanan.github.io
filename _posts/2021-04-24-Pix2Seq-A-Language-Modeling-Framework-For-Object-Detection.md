---
layout: post
title: Pix2Seq
categories: Computer Vision
tags:
mathjax: true
comments: true
---


[Chen et al, 2021](https://blog.research.google/2022/04/pix2seq-new-language-interface-for.html) present Pix2Seq where object detection is cast as a language modeling problem. Typically, in language modeling the loss function is designed so that the model predicts the next word in a sequence given all the previous words. 

The authors build on this idea and ask "can each object be interpreted as a token and the model outputs a sequence per image, containing several tokens (one per object)?"

Here the authors additionally condition the prediction of the next token on all the previous other tokens in the image and the image content itself. Each token here is an object bounding box description.

The authors say that typically the coordinates of the bounding box live in a continuous space. However for language modeling, the tokens are supposed to be discrete. Hence, the coordinates are mapped to a bin and the complete vocabulary then is a list of $N$ bins per image. 
Since each image can have an arbitrary number of objects, there needs to be an EOS (end of sequence) once all objects are predicted. Additionally, since there is no distinct ordering of tokens in the sequence, the authors experimented with randomly presenting objects in an image and stated that this strategy gave the best results. In this work, the authors use a typical (convolutional) encoder but a transformer decoder so that each token can compute the cross attention to all other tokens in the image. 

I wonder if this can be extended to the task of tracking, where given an initial starting segmentation in one image (much like how humans track objects where they follow one object across time and are bad at observing multiple tracks parallelly), one interprets a tracklet as a sequence of tokens, where each token is the same object but seen at a different time point. This would enable computation of cross attention of an object at $t$ to all objects at $t+1$ and can be used to solve for the complete lineage tree during inference. Also, unlike the problem of object detection, where EOS indicates that all tokens have been detected, here the end of a sequence could indicate cell death or maybe used for indicating mitosis. 

One issue I see with this strategy is that since all tracklets have different lengths, then one might need to condition the prediction of the next token on a varying number of previous images. But maybe this can be approximated by conditioning on a time window of fixed length. 

The benefit of introducing language modeling in the context of cell tracking is that each token or object can attend to objects at much later time points. This is typically missing in approaches which rely on predicting flow between consecutive pair of frames.
