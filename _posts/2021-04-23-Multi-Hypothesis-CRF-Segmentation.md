---
layout: post
title: Multi-Hypothesis CRF Segmentation
categories: Computer Vision
tags:
mathjax: true
comments: true
---

[Funke et al, 2011](https://arxiv.org/abs/1109.2449) state that the existing approaches for neuron segmentation fall into two categories. The former try to address this as a binary segmentation problem where the outlines of neurons are identified, and then geometrically consistent assignment is established to identify connected components belonging to one neuron. The second set of approaches emphasize over segmentation and then merge small regions within and betwen slices. The authors' approach aligns with the first set of approaches. They provide a method for incorporating rivaling sgemnetation hypotheses.

Their assignment model is explained as follows. Each possible continuation of a hypothesis $C^{i}$ in slice $z$ to a hypothesis $C^{j}$ in slice $z+1$ is represented by variable $a_{ij}$. Similarly, a split of $C_{i}$ in slice $z$ to $C_{j}$ and $C_{k}$ in slice $z+1$ is represented as $a_{i, jk}$. Similarly merge is shown as $a_{ij, k}$. Appearances are shown as $a_{E, i}$ and disappearances are shown as $a_{i, E}$. 

The authors introduce a couple of consistencies - (i) Hypothesis consistency which suggests that no pixel is assigned to more than one segmentation hypothesis and (ii) Explanation consistency which suggests that for each segmentation hypothesis, the sum of incoming assignments has to be equal to the sum of outgoing assignments.  

What escapes me at the moment is how to modify the method in case the segmentation hypotheses come not by changing a threshold but from different methods. In the current formulation, it seems that all segmentation hypotheses are arranged hierarchically. 
Another question (probably since I don't understand it fully yet) is in the case of splits, how is the explanation consistency guaranteed, since number of incoming assignments might be one but number of outgoing assignments could be two or more.


