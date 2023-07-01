---
layout: post
title: An exact hypergraph matching algorithm
categories: Biology
tags:
mathjax: true
comments: true
---
(From [Lauziere _et al_, 2022](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0277343))
>"Analyses in late-stage development are complicated by bouts of rapid twitching motions which invalidate cell tracking approaches. However, the embryo possesses a small set of cells which may be identified, thereby defining the coiled embryo's posture." 

Lauziere _et al_, 2022 address the problem of cell tracking for _C. elegans_ in the presence of bouts of animal twitching. They say that the seam cells (22) can be easily labeled through imaging techniques and are sufficient to determine the posture of the worm. They say that through exact hypergraph matching algorithm, they were able to show that 56 % of the times they were able to recover all the 22 seam cells correctly, and I guess other times, the algorithm did not converge to this solution. 

>"Graphs are limited in their expressive power as edges can only relate pairs of vertices at a time; hypergraphs extend the definition of a graph to include hyperedges which can specify relationships between an arbitrary number of vertices"

The authors propose to do hypergraph matching, in which one doesn't just compare edges (as done in graph matching) but considers triplets or higher order number of vertices to perform matching between two sets of nuclei. 

Since the problem at hand has only 22 vertices which need to be correctly matched, it makes the use of hypergraphs feasible. The key aspect about the data is that in order to preserve the embryo health, the worm is imaged every five minutes. But in this duration of five minutes, there is a complete repositioning of the embryo. 
 
The authors use the software MIPAV which allows data annotation and implements an older version of their method for straightening of the worms. The bigger picture behind the straightening of the worms is that downstream tracking of the 900 or so neurons is easier to do in the straightened worm space (although they don't actually quantify the improvement in tracking, enabled by their method for node matching, probably because it is only accurate 56 % of the times).

 
