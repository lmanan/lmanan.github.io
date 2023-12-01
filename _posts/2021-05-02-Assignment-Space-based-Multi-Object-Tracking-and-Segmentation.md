---
layout: post
title: Assignment-Space-based Multi-Object Tracking and Segmentation 
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Choudhuri et al, 2022](https://openaccess.thecvf.com/content/ICCV2021/papers/Choudhuri_Assignment-Space-Based_Multi-Object_Tracking_and_Segmentation_ICCV_2021_paper.pdf)) 
>"Classical global methods on tracking operate directly on object detections, which leads to a combinatorial growth in the detection space. In contrast, we formulate a global method for MOTS over the space of assignments rather than detections: First, we find all top-k assignments of objects detected and segmented between any two consecutive frames and develop a structured prediction formulation to score assignment sequences across any number of consecutive frames. We use dynamic programming to find the global optimizer of this formulation in polynomial time."

Choudhuri et al state that performing a global optimization considering detections obtained at each time point is computationally very expensive. On the other hand, performing a greedy association by considering pairs of consecutive time points would lead to many errors. So a middle ground would be to consider k assignments per pair of time frames and then performing a global optimization over the assignment space. Figure 2 taken from their paper summarizes this well. 

<p><figure><img src="/images/2021-05-02/top-k.png" alt=""/></figure></p>


