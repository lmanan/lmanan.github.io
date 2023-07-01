---
layout: post
title: Global Shape Descriptors for Segmentation 
categories: Computer Science
tags:
mathjax: true
comments: true
---


(From [Kervadec et al, 2021](https://openreview.net/pdf?id=nqe6e0oJ_fL))
>"Not only interesting theoretically, there exist deeper motivations to posing segmentation problems as a reconstruction of shape descriptors: First, annotations to obtain approximations of low-order shape moments could be much less cumbersome than their full-mask counterparts, and anatomical priors could be readily encoded into invariant shape descriptors, which might alleviate the annotation burden."

Kervadec et al, 2021 propose an interesting strategy for performing semantic segmentation. Instead of updating network weights using the cross entropy loss and framing the problem of semantic segmentation as one of pixel classification, the authors say that the weights should be updated using deviation from some global shape descriptors. There are multiple reasons to go along this route - one, that it is much easier to calculate these global shape descriptors without annotating a dense mask. Two, since this is a parametric representation of the shape, it is far more interpretable. Three, it is easy to encode some anatomical priors (say regarding the min and max length of an organ) within these shape descriptors. Four, that these shape descriptors might often be invariant across different imaging protocols etc, which might open interesting avenues for generalization in segmentation. 

>"Informally, we could say that the existing segmentation methods are micro-managing pixels, taking each as a separate classification problem, instead of supervising the global shape information of segmentation prediction."

In order to explain these global shape descriptors, the authors begin by introducing shape moments and central moments:

$$
\mu_{p,q}^{(k)}(s_{\theta}) := \sum s_{\theta}^{(i,k)} x^{p}_{(i)}y^{q}_{(i)},
$$

where $p$ and $q$ are the moment orders. Here, $_{\theta}^{(i,k)}$ indicates the soft-max probability for pixel $i$ and class $k$. 

Similarly, the central moment is defined as:

$$
\bar{\mu}_{p,q}^{(k)} := \sum s_{\theta}^{(i,k)} \left( x_{(i)} - \frac{\mu_{1,0}^{(k)}}{\mu_{0,0}^{(k)}} \right)^{p}  \left( y_{(i)} - \frac{\mu_{0,1}^{(k)}}{\mu_{0,0}^{(k)}} \right)^{q}
$$

Next, the authors describe some shape descriptors:

* Volume

$$
B^{k}(s_{\theta}) :=  \mu_{0,0}^{k}(s_{\theta})
$$

* Centroid

$$
C^{k} (s_{\theta}) := \left(\frac{\mu^{k}_{1,0}(s_{\theta})}{\mu^{k}_{0,0}(s_{\theta})}, \frac{\mu^{k}_{0,1}(s_{\theta})}{\mu^{k}_{0,0}(s_{\theta})}  \right)
$$


* Length

$$
L^{k} (s_{\theta}) := \sum |s_{\theta}^{(i,k)} - s_{\theta}^{(j,k)} | L_{\omega, i, j}
$$

>"Very surprisingly, as little as 4 descriptors per class can approach the performance of a segmentation mask with 65k individual discrete labels."

One weakness of this appproach is that it requires the whole image to be shown - the authors mention this and state that the image can not be patched. They also say that they barely scratched the surface and possibly higher order moments could be used. Separately, if some invariance is known apriori, then that such invariant shape descriptors could be constructed. 


An application of this work is seen in a follow up work by [Bateson et al, 2022](https://arxiv.org/pdf/2205.07983.pdf):

>"This motivates Domain Adaptation (DA) methods: DA amounts to adapting a model trained on an annotated source domain to another target domain, with no or minimal new annotations for the latter. Popular strategies involve minimizing the discrepancy between source and target distributions in the feature or output spaces; integrating a domain-specific module in the network; translating images from one domain to the other; or integrating a domain-discriminator module and penalizing its success in the loss function."


Bateson et al, 2022 try to address out of distribution performance to test data. They say that due to variation in imaging modalities and protocols, one is usually forced to label a number of images in the target (test) domain. The focus of domain adapatation is to minimize the discrepancy between the source (training) and target (test) distributions without any additional annotations on the latter. 

The authors initially train a model on the training data with normal cross entropy loss. Next, the weights are frozen and the scale and bias of the batch normalization layer are optimized on each individual test image by a modified loss function which minimizes the discrepancy of the predicted global shape descriptors on the test image and the average global shape descriptors on the training images. This leads to SOTA results on OOD test data.


