---
layout: post
title: Learning Shape Abstractions
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-05-05/sr.png" alt=""/><figcaption>
[Source: Tulsiani et al, 2017. Examples of chair and animal shapes assembled by composing simple volumetric primitives (cuboids). The obtained reconstructions allows an interpretable representation for each object and provides a consistent parsing across shapes]</figcaption></figure></p>

(From [Tulsiani et al](http://openaccess.thecvf.com/content_cvpr_2017/papers/Tulsiani_Learning_Shape_Abstractions_CVPR_2017_paper.pdf), 2017)
>"The motivation for this parametrization is to exploit the compositionality of parts as well as the independence of ‘what’ and ‘where’ (part shape and spatial transformation respectively).  The representation of a shape as a set of parts allows independent reasoning regarding semantically separate units like chair legs, seat etc. The decomposition in terms of part shape and transformation parameters further decomposes factors of variation like ‘broad aeroplane wing’ (captured by shape) and ‘tilted chair back’ (captured by transformation)."

Tulsiani and colleagues provide a novel strategy to represent a volumetric shape as a composition of volumetric primitives (*cuboids*). The authors state that such a parameterization allows decoupling the questions of *what constitutes an object* and *where do these primitive cuboids end up*. The learnt representations are also consistent across multiple poses of the same object i.e. the same primitive cuboid is responsible, say for the head portion of the deer in the image above. 

The authors state that the main reason why they succeed and the classical approaches fail is because they attempt to explain the entire dataset jointly, thus allowing them to learn the common 3d patterns from the data. This makes me think that say if this strategy were to be extended to biological data, then one could try explaining each and every different biological cell as a variant of a certain composition of primitives (ellipses).

The authors express each primitive encoded in terms of a tuple $(z, q, t)$ where $z$ represents its shape in a canonical frame and (q,t) represent the spatial trotation and translation respectively. Next they use a loss comprising of two terms - *consistency loss* which explain how well is the shape abstraction subsumed by the object and *coverage loss* which explains how well is the object subsumed by the shape abstraction.

>"An aspect for computing gradients for the predicted param-eters using this loss is the ability to compute derivatives for $$z_{m}$$ given gradients for a sampled point on the canonical untransformed primitive $$p^{'} \sim S(P_{m})$$. We do so by using the re-parametrization trick which decouples the parameters from the random sampling. As an example, consider a point being sampled on a rectangle extending from $$(−w,−h)$$ to $$(w, h)$$. Instead of sampling the x-coordinate as $$x \sim [−w, w]$$, one can use $$u \sim [−1,1]$$ and $$x=uw$$. This re-parametrization of sampling allows one to compute $$\frac{\partial x}{\partial w}$$."

The authors use the clever reparameterization trick in order to sample the surface of the object and decouple the parameters $z$ from the random sampling. More details [here](https://shubhtuls.github.io/volumetricPrimitives/appendix.pdf). Asecond crucial contribution is having a variable number of components and providing a stochastic output parameter that explains the probability of whether the primitive exists or not.
