---
layout: post
title: Gromov-Wasserstein Averaging of Distance Matrices 
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-04-29/barycenter.png" alt=""/><figcaption>
   [Source: Peyre et al, 2016. Barycenters of point clouds from MNIST digits. Sample input point clouds are shown on the left and a representation of the barycenter distance matrix is shown on the right]</figcaption></figure></p>


(From [Peyre et al](http://proceedings.mlr.press/v48/peyre16.pdf), 2016)
>"A  major  difficulty  with  this  (pair wise distance matrices) representation  is  that  in many  applications  these  matrices  are  not  registered  or aligned, meaning that there is no explicit correspondence between their rows and columns. This is the case for shapes that  undergo  pose  variation,  articulation  or  deformation. Even worse, in some cases, similarities are not defined over the same ground space.  For example, different molecules may have varying numbers of atoms. This inconsistency yields matrices of varying dimensions."


Peyre and colleagues provide a computational framework for summarizing sets of unaligned pair-wise distance matrices. They employ Gromov-Wasserstein distances between matrices *while* building interpolants and barycenters which inherit structure from the inputs.

Given some cost matrix $$c \in R_{+}^{N_{1} \times N_{2}}$$ where $$c_{ij}$$ represents the transportation cost or the ground distance between bins $$p_{i}$$ and $$q_{j}$$, the authors define the solution of entropically-regularized optimal transport between these two histograms $p$ and $q$ as

$$ T(c, p, q) := \text{arg min} <c, T> - \epsilon H(T) $$ 

Here <., .> denotes the Frobenius norm and $$ H(T) := - \Sigma_{i=1}^{N_{1}} \Sigma_{j=1}^{N_{2}} T_{i,j} \left( \log T_{i,j} - 1 \right) $$.

There is a clean solution to the above which was shown in [Cuturi](https://arxiv.org/abs/1306.0895) and it reads $$ T(c, p, q) = \text{diag}(a) K \text{diag}(b) $$ where $$ K := \exp^{-\frac{c}{\epsilon}} \in R_{+}^{N_{1} \times N_{2}} $$ and $$a$$ and $$b$$ can be computed by Sinkhorn iterations:

$$ a_{\text{new}} = \frac{p}{Kb_{\text{old}}}$$

$$ b_{\text{new}} = \frac{q}{K^{T} a_{\text{old}}}$$

Would be interesting to see how this above fares with the use of shape context as unary features for object matching. Next, the authors address the question of how the above is extended for estimating the discrepancy between two pair-wise distance matrices. They employ the Gromov-Wasserstein discrepancy between two measured similarity matrices $$C$$ and $$\bar{C}$$ as follows:

$$ \text{GW} ( C, \bar{C}) := \text{min} \varepsilon_{C, \bar{C}} (T)$$

$$\varepsilon_{C, \bar{C}} (T) := \Sigma_{i,j,k,l}  L(C_{i,k} \bar{C}_{j,l}) T_{i,j} T_{k,l} $$

Here the Loss Matrix $$L$$ is chosen as the KL-divergence and is defined as $$L(a,b) : = = a \log(\frac{a}{b}) - a +b $$ while the matrix $$ T$$ is the coupling or flow between the two spaces on which the similarity matrices are defined (this the unknown which needs to be iterated over, in order to determine the GW discrepancy).

The authors further show that the barycenter can be jointly estimated while softly assigning the probability of matching between the elements of the point clouds in question. 
