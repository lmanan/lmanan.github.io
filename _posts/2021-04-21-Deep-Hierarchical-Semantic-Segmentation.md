---
layout: post
title: Deep hierarchical semantic segmentation
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Li et al, 2022](https://arxiv.org/abs/2203.14335))
>"The vast majority of modern segmentation models simply assume that all target classes are disjoint and should be distinguished exclusively during pixel-wise prediction. This fails to capture the structured nature of the visual world: complex scenes arise from the composition of simpler entities. Walking city, vehicles and pedestrians fill our view. After focussing on the vehicles, we identify cars, buses and trucks, which consist of more fine-grained parts like wheel and window. On the other hand, structured learning of our world in terms of relations and hierarchies is a central ability in human cognition. We group chair and bed as furniture., while cat and dog as pet. We understand this world over multiple levels of abstraction, in order to maintain stable, coherent percepts in the face of complex, visual inputs."

Li et al say that a vast majority of classification -targetted methods assume that classes are disjoint, but this fails to capture the structured nature of the visual world. In their work called HSSN, classes are not organized in a flat structure, but in a tree-shaped hierarchy. 

>" Thus each pixel observation is associated to root-to-leaf path of the class hierarchy capturing general to specific relations between classes. "

Their contributions were:

1. Unlike previous works which enforce hierarchy through network designs, they formulate HSS as a pixel wise multi label segmentation task.
2. To make pixel predictions coherent with class hierarchy:
    - a pixel sample belonging to a given class must belong to all ancestors
    - a pixel sample not belonging to a given class must not belong to its descendants
3. HSSN encodes structured knowledge introduced by the class hierarchy into the embedding space.

What was quite intriguing to me was the Tree Triplet Loss introduced by the authors. The authors motivate this loss by saying that:

>"As the class hierarchy provide rich semantic relations among categories over different levels of concept abstraction, next we will exploit such structured knowledge to reshape the pixel embedding space $f_{enc}$, so as to generate more efficient pixel representations and improve final segmentation performance"

The authors state that for any pairs of labels $u$ and $v$, ley $\psi(u, v)$ denote the distance in the tree - i.e. the length in edges of the shortest path between $u$ and $v$ in $T$. This distance function is non-negative, $\psi(v,v) =0$, $\psi(u,v) = \psi(v,u)$ and the triangle inequality always holds. 

It would be interesting to apply this idea to biological data where ground truth lineages showing relations between parents and children are available. 
A simple idea could be to query pairs of segmentations from the lineage tree, learn a feature per object for each segmentation and have a loss where for a pair of segmentations, the distance in the feature space is the length of the shortest path in the lineage space. 

Along with the feature per segmentation, the global positional and time sinusoidal encoding could be additionally concatenated 

