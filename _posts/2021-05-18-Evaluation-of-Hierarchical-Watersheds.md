---
layout: post
title: Hierarchical Watersheds
categories: Computer Vision
tags:
mathjax: true
comments: true
---

From [Perret et al, 2018](https://hal.science/hal-01430865v4/document)

>"This article aims to understand the practical features of hierarchies of morphological segmentations, namely the quasi-flat zones hierarchy and watershed hierarchies, and to evaluate their potential in the context of natural image analysis."

What are the quasi-flat zones?

>"We show that in conjunction with a state of the art contour detector, watershed hierarchies are competitive with complex state of the art methods for hierarchy construction".

Why are watershed based hierarchies compatible only with contour detectors? Why not something that begins from the object center and grows outwards. 
What other approaches are there for hierarchy construction?

>"This enables defining hierarchies of watershed whose minima are iteratively removed according to an importance measure for e.g.  related to their object sizes"

What does it mean to say that minima are iteratively removed?

>"There exist efficient algorithms with the same time complexity as minimum spanning trees algorithms, to construct them enabling to process large images in real time."

What is the time complexity of minimum spanning tree algorithm?

"Hierarchies of watersheds are thus multi-scale representations which satisfy a global optimality property."

What global optimal property are the authors referring to? Global and optimal with respect to what?

>"Given a weighted graph and a family of markers (i.e. a subset of the graph vertices identifying the objects of interest), the problem of the minimum spanning forest is to find a spanning forest of minimum total weight, defined as the sum of the weights of its edges, such that each connected component of the forest contains exactly one marker."

This is a good definition of the problem! It is basically a constrained graph cut problem. Constrained because each connected component must contain one marker.

>"If the markers are ranked, say according to an importance measure, it is possible to obtain a sequence of nested minimum spanning forests such that the kth minimum spanning forest is rooted in the kth most important markers. A usual choice to define a sequence of markers is to rank the minima of the weight map according to extinction values."

What exactly are extinction values?


