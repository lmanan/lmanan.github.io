---
layout: post
title: Learning Graph Matching
categories: Computer Vision
tags:
mathjax: true
comments: true
---
(From [Caetano et al](https://cs.stanford.edu/~quocle/CaeMcLiLeSmola.pdf), 2009)
>"An interesting question arises in this context.  If we are given  two  attributed graphs, G and G′,  should  the  optimal match be uniquely determined?  For example, assume first that G and G′ come from two images acquired with a surveillance camera in an airport’s lounge. Now, assume the same G and G′ instead come from two images in a photographer’s image database.  Should the optimal match be the same in both situations? If the algorithm takes into account exclusively the graphs to be matched, the optimal solutions will be the same since the graph pair is the same in both cases. This is how graph matching is approached today. In this paper, we address what we believe to be a limitation of this approach. We argue that if we know the “conditions” under which a pair of graphs has been extracted, then we should take into account how graphs arising in those conditions are typically matched."

Caetano et al argue that one should take into account the conditions under which two graphs have been extracted, by learning from user-annotated pairs. They further show that such learning can enhance the results of the optimization. 

>"Define a matching matrix $$y$$ by $$y_{ii} \in {0,1}$$ such that $$y_{ii'}$$ = 1 if node $$i$$ in the first graph maps to node $$i'$$ in the second graph and $$y_{ii'} = 0$$ otherwise. Define by $$c_{ii'}$$ the value of the compatibility function for the unary assignment  and by $$d_{ii'jj'}$$ the value of the compatibility function for the pairwise assignment. Then, a generic formulation of the graph matching problem consists of finding the optimal matching matrix y* given by the solution of the following (NP-hard) quadratic assignment problem".

$$ y^{*} = \text{argmax} \left[ \sum_{ii'} c_{ii'} y_{ii'} + \sum_{ii'jj'} d_{ii'jj'} y_{ii'}y_{jj'} \right] $$

The associated loss used is the Hamming Loss which penalises the number of mismatches between the predicted and target matrices $$y$$ and $$y^{n}$$. I wonder if such a case of a few sparse parameters can be learnt using Gradient Descent in pytorch. How far is the local obtained minima from the global minima? Which unary feature should one use for three-dimensional data? Also, how should the edges be constructed in a graph corresponding to an image where there are no visible edges - would a fully connected network help perhaps?
