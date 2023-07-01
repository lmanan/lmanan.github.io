---
layout: post
title: Discovering Objects that can move
categories: Biology
tags:
mathjax: true
comments: true
---

(From [Bao et al, 2022](https://arxiv.org/pdf/2203.10159.pdf))
>"What makes this task especially challenging is that the notion of an object is inherently ambiguous and context-dependent. Consider a car: its left door and the handle on that door can be treated as individual objects, or parts of the whole. It is thus not surprising that existing approaches that attempt to automatically separate objects from the background based on appearance struggle beyond controlled scenarios. In particular, classical methods that use graph-based inference tend to over or under-segment the objects"

Bao _et al_, 2022 argue that appearance based approaches would under-perform on the task of object discovery  in an image because the _notion_ of the object is very context dependant. They say that a handle of a door's car can be considered an object but in most contexts, one would want the entire car be segmented as a whole. Hence, they motivate their work by saying that pixels that move together independently, should be considered as constituting individual objects. 

>"We posit that while the ambiguity of the object definition is not resolvable in the static image world without direct supervision, it has a natural resolution in the dynamic world of videos. Concretely we choose to focus on dynamic objects, which we define as entities that are capable of moving independently in the world. Independent object motion is a strong grouping cue, which has been shown to drive object learning in animal perception."

In one of the related works (Pathak _et al_, 2016), the authors say thay both infants and newly sighted congenitally blind people tend to oversegment static objects, but can group things properly when they move. 

(From [Pathak _et al_, 2016](https://arxiv.org/pdf/1612.06370.pdf))
>"To do so, they may rely on the Gestalt principle of common fate: pixels that move together tend to belong together."

I wonder why the authors refer to the problem as one of _object discovery_ and not that of _instance segmentation_. Also, I feel that unsupervised object discovery using motion cues  makes a lot of sense especially in the context of biological data, where appearance cues on static images might be confusing in dense regions but by incorporating motion data from a few neighboring frames, one can disambiguate the underlying objects. 
I wonder how pretrained models like _RAFT_ do on time lapse microscopy movies, for estimating flow.

Also, identifying the rough outline of an object based on flow calculation between neighboring frames (as is done by Pathak _et al_) would work, but it is not obvious how easy it would be to extend this for the task of instance segmentation (i.e. segmenting multiple objects based on their individual flow patterns) probably.




