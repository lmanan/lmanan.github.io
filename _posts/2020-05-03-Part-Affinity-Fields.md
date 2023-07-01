---
layout: post
title: Part Affinity Fields 
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-05-03/paf.png" alt=""/><figcaption>
[Source: Cao et al, 2017. The method takes the entire image as input for a two-branch CNN to jointly predict confidence maps for body part detection (b) and part affinity fields for part association (c). The parsing step happens separately to associate body part candidates. The final assembled full body poses for all people is shown in (e)]</figcaption></figure></p>



(From [Cao et al](https://arxiv.org/pdf/1611.08050.pdf), 2017)
>"A  common  approach  is  to  employ a person detector and perform single - person pose estimation  for  each  detection.   These  top-down  approaches  directly  leverage  existing  techniques  for  single-person  pose estimation,  but suffer from early commitment:  if the person detector fails – as it is prone to do when people are in close proximity – there is no recourse to recovery.  Furthermore, the runtime of these is proportional to the number of people:  for  each  detection,  a  single-person  pose  estimator  is run, and the more people there are, the greater the computational cost. In contrast, bottom-up approaches are attractive as they offer robustness to early commitment and have the potential to decouple runtime complexity from the number of people in the image.  Yet, bottom-up approaches do not directly use global contextual cues from other body parts and  other  people.   In  practice,  previous  bottom-up  methods do not retain the gains in efficiency as the final parse requires costly global inference. "


Cao et al mention that top down approaches which detect individuals first, and then determine the keypoints suffer from an *early commitment* while the bottom-up approaches, just by themselves, may suffer from missing out on the global context. Additionally, some of the earlier botton up works that jointly labeled part detection candidates and associated them to multiple people, needed to solve an integer linear programming problem over a fully connected graph, which was slow. The authors therefore come up with a new bottom up approach employing *part-affinity fields*. These part affinity fields encode the orientation and location of limbs, provide a novel strategy to associate pair-wise affinity between parts and allow for a greedy parse which deliver good scores at a low computation cost.

The system takes an image as input and outputs a set $\{S_{j} \}$ of 2D confidence maps $j \in \{1, \ldots, J \}$ for the J parts and a set $\ {L_{c} \}$ of 2D part affinity field $c$ in $\{1, \ldots, C \}$ for the C *limbs* (or part pairs)! The loss function for these predictions are Mean Squared Error. The ground truth for the part detection is defined through a decaying exponential function. Say if $x_{j,k} \in R^{2}$ be the ground truth position of body part $j$ for person $k$ in the image, then the value at location $\mathbf{p} \in R^{2}$ is:

$$ \mathbf{S}^{*}_{j,k}(\mathbf{p}) = \exp \left( \frac{- \vert\vert \mathbf{p} -\mathbf{x}_{j,k}\vert \vert^{2}_{2}}{\sigma^{2}}\right)$$

For multiple persons, the ground truth confidence map is an aggregation of individual confidence maps via a max operator. 

The ground truth part affinity vector field $$L^{*}_{c,k}$$ at an image point $p$ is:

$$ \mathbf{L}^{*}_{c,k}(\mathbf{p}) = \mathbf{v} \text{ if p on limb c, k}$$

$$ \mathbf{L}^{*}_{c,k}(\mathbf{p}) = \mathbf{0} \text{ otherwise}$$

Here, $$\mathbf{v} = \frac{x_{j2,k} - x_{j1, k}}{\vert \vert x_{j2, k}- x_{j1, k}\vert\vert_{2}}$$

How do we know if  a point $\mathbf{p}$ is on a limb? The set of points on the limb is defined as those within a certain distance threshold of the line segment connecting the two ground truth poart positions i.e. those points $p$ for which:

$$ 0 \leq \mathbf{v}. (\mathbf{p} -\mathbf{x}_{j1, k}) \leq l_{c,k}$$
$$ \vert \mathbf{v}_{\bot}. (\mathbf{p} - \mathbf{x}_{j1, k}) \vert \leq \sigma_{l}$$


What I found was the most interesting bit was how the authors measure the association between two parts using the part affinity field! This is accomplished by computing the line integral over the corresponding part affinity field along the line segment connecting the candidate part locations. As the authors put it:

>"In other words, we measure the alignment of the predicted PAF with the candidate limb that would be formed by connecting the detected body parts."
