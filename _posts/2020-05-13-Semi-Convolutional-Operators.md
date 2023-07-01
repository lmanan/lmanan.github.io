---
layout: post
title: Semi-convolutional Operators
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-05-13/semiconvolutional.png" alt=""/><figcaption>
[Source: Novotny et al, 2017. An instance segmentation pixel embedding is trained for a synthetic training image consisting of a regular dot pattern (a). After training a model on that image, the produced embeddings are clustered using k-means encoding the corresponding cluster assignments with consistent pixel colors. A standard convolutional embedding (c) can not successfully embed each dot into a unique location due to its translational invariance. The proposed semi-convolutional operator (d) naturally embeds each dot with identical appearance but distinct location into distinct regions in the feature space and hence allows successful clustering of the instances]</figcaption></figure></p>


[Novotny et al ](http://openaccess.thecvf.com/content_ECCV_2018/papers/Samuel_Albanie_Semi-convolutional_Operators_for_ECCV_2018_paper.pdf) make an interesting observation that owing to the translational invariance of the fully convolutional neural networks, regions with similar appearance but disparate locations might activate similar filters and hence, be embedded with a similar tag or vector. To counter this, they propose to make the network semi convolutional by predicting for each pixel its displacement vector to the centre of the object it belongs to and combining it with the pixel location through a simple linear, additive function. Since the pixel location is unique, this breaks the notion of translational invariance. Next, the authors show results on the city scape dataset and the challenging BBBC10 *C. elegans* dataset. The latter is one that I am interested in, since it contains shapes which are non-convex.

>"In practice, very rarely an image contains exact replicas of a certain object. Instead, it is more typical for different occurrences to have some distinctive individual traits. For example, different people are generally dressed in different ways, including wearing different colors. In instance segmentation, one can use such cues to tell right away an instance from another. Further more, these cues can be extracted by conventional convolutional operators. In order to incorporate such cues in our additive semi-convolutional formulation, we still consider the expression $$\psi_{u}(x) = \hat{u} +\phi_{u}(x)$$. However, we relax $$\psi_{u}(x) \in R^{d}$$ to have more than two dimensions $$d >2$$. Furthermore, we define $$\hat{u}$$ as the pixel coordinates of $$u$$,$$u_{x}$$ and $$u_{y}$$, extended by zero padding:
$$ \hat{u} = [ u_{x}, u_{y}, 0 \ldots 0 ]^{T} \in R^{d}$$ In this manner, the last $$d-2$$ dimensions of the embedding work as convolutional features and can extract instance-spiecifc traits normally."

I liked how the authors produce a higher dimension embedding for each pixel, where the first two units are reserved for its displacement in x and y from the effective centre of the object to which it belongs, while the remaining units can encode instance-specific traits. 
