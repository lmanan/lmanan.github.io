---
layout: post
title: Registering Two Time Lapse Movies
categories: Computer Vision
tags:
mathjax: true
comments: true
---
(From [Guignard et al, 2014](https://hal.inria.fr/hal-00919142/file/Article_Leo.pdf))
>"Recent progress in light microscopy allows the capture of 3D images of entire live embryos at a sub-cellular level of resolution with a high rate of acquisition and without interfering with development. These protocols produce 3D+t images depicting the development. Comparing or fusing such sequences captured from
different embryos is a challenging task, but of crucial importance. While it is generally not possible to image an embryo throughout its whole development, one can generate developmental sequences from different individuals, with partial temporal overlap. Stitching together such time series can be used to produce a consensual representation of the whole development, and would also allow to identify variations in these programs."

Guignard et al propose a technique to align two movies without counting the number of nuclei precisely. It involves identifying some "landmarks" in each movie and then finding an optimal  pairings between using these sets of 3D+t landmarks to align two independent sequences.

The authors explore *Phalusia mammillata* which develops in a stereotypic manner, and for which cell identities are known up to the 112 cell stage. They explored two different specimens which were imaged at different temperatures - 20 and 18 degree celsius, hence the development kinetics were different, even though the two embryos started at the same developmental stage. The authors chose the block matching scheme which they claim is robust to many topological changes. 

My general question, not knowing much about the block matching sceheme currently is whether it could also end up in local minima, similar to the iterative closest point. Additionally, the authors state that the block matching scheme utilizes iconic primitives - what are these, how are they defined, is the block size pre-defined or is it rather explored in a scale-space manner?

The advantage with using the block matching scheme is that one does not need to know to detect the nuclei as point clouds, but one could rather be in the image pixel space. The authors estimate an iterative rigid transform and additionally use the least trimmed square method to discard outlier pairings. I wonder how the least trimmed square compares with the use of RANSAC!

By leveraging the correspondences obtained using a linear, rigid model, a non-linear transform is estimated. The authors have the following to state about that procedure: 

>"The non-linear transformation is represented by a dense vector field. The transformation update follows then the following steps: 1. the sparse pairing field (cR, cF) is transformed into a dense field by Gaussian interpolation (this Gaussian interpolation also acts as a fluid regularization); 2. outlier pairings are discarded; 3. the remaining pairings are transformed into a dense field by Gaussian interpolation; 4. this dense field is composed with the transformation foundat the previous iteration; 5. the resulting transformation is smoothed by a second Gaussian filter that acts as an elastic regularization."

While investigating two independent sequences, the authors determined landmarks which were regions of high deformation (velocity) and performed an association between the two sets of landmarks acquired from these two sequences, under some constraints for example that pairings are temporally consistent, which appears like quite a logical idea to me.

In order to validate the registration strategy, the authors chose the amount of overlap between the membranes as a loss metric. I wonder why they did not also mention the number of nuclei between estimated corresponding frames from two independent sequences, additionally as a validation measure?

 


