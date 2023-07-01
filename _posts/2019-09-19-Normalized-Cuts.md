---
layout: post
title: Normalized Cuts
categories: Computer Vision
tags:
mathjax: true
comments: true
---
(From [Shi and Malik, 2000](https://people.eecs.berkeley.edu/~malik/papers/SM-ncut.pdf))
> "Since there are many possible partitions of the domain of an image into subsets, how do we pick the right one? There are two aspects to be considered here. The first is that there may not be a single correct answer. A Bayesian view is appropriate – there  are  several  possible  interpretations  in the  context  of  prior  world  knowledge.  The  difficulty,  of course, is in specifying the prior world knowledge. Some of it  is  low  level,  such  as  coherence  of  brightness,  color, texture,  or  motion,  but  equally  important  is  mid -  or  high - level   knowledge   about   symmetries   of   objects   or   object models.  The   second   aspect   is   that   the   partitioning   is inherently  hierarchical.  Therefore,  it  is  more  appropriate to  think  of  returning  a  tree  structure  corresponding  to  a hierarchical partition instead of a single flat partition. This  suggests  that  image  segmentation  based  on  low - level cues cannot and should not aim to produce a complete final  correct segmentation.  The  objective  should  instead be to use the low - level coherence of brightness, color, texture, or motion   attributes   to   sequentially   come   up   with   hierarchical partitions.  Mid -  and  high - level  knowledge  can  be  used  to either   confirm   these  groups   or  select   some  for  further attention. This attention could result in further repartitioning or grouping. The key point is that image partitioning is to  be  done  from  the  big  picture  downward,  rather  like  a painter first marking out the major areas and then filling in the details"


Shi and Malik argue that partitioning an image is an inherently, heirarchical task and should return a tree-like structure and not a flat partition. Low level features such as intensity, texture et cetera should generally not be used, by themselves, to produce a final segmentation map. Rather they should serve as input for the next level of grouping. The authors also reckon that many existing segmentation criteria, in practice, are based on local properties of the image or graph: this goes against the notion of **perceptual grouping**, which is based on extracting global impressions of a scene, and therefore these segmentation algorithms fall short in achieving their goal. 
