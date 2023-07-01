---
layout: post
title: Earth Mover Distance
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-04-28/rubner.png" alt=""/><figcaption>
   [Source: Rubner et al, 2000. An example where bin-by-bin L1 distance does not match perceptual dissimilarity]</figcaption></figure></p>



(From [Rubner et al](https://www.cs.cmu.edu/~efros/courses/LBMV07/Papers/rubner-jcviu-00.pdf), 2000)
>"These (bin-by-bin dissimilarity) measures do not necessarily match perceptual similarity well. A major drawback of these measures is that they account only for the correspondence between bins with the same index, and do not use information across bins. This problem is illustrated in figure 1 (a) which shows two pairs of one-dimensional gray-scale histograms. For instance, the L1 distance between the two histograms on the left is larger than the L1 distance between the two histograms on the right, in contrast to perceptual dissimilarity. The desired distance should be based on correspondences between bins in the two histograms and on the ground distance between them as shown in part (c) of the figure. 
"

It appears to me that Rubner and colleagues were among the first ones to promote the use of Earth Mover's distance (EMD) in the field of computer vision. Among others, one advantage with using the Earth Mover's Distance is that it allows having a *signature* or *histogram* which is defined over a smaller bin width. The pre-requisite to using EMD is defining apriori a *ground-distance* which explains how much effort is it to move *material* between different bins. The use of EMD should be more robust (than other bin-to-bin dissimilarity measures, for example) to noisy bin values.

Evaluation of EMD is evaluated in the following linear programming problem. Let $$p_{i}$$ denote the bin $$i$$ in the first histogram and $$q_{j}$$ denote bin $$j$$ in the second histogram.  $$d_{ij}$$ is the ground distance between bins $$p_{i}$$ and $$q_{j}$$. We want to find a flow $$f_{ij}$$ between $$p_{i}$$ and $$q_{j}$$ that minimizes the overall cost.

$$ \text{work} = \Sigma^{m}_{i=1} \Sigma^{n}_{j=1} d_{ij}f_{ij} $$

subject to the following constraints:

$$f_{ij} \geq 0 $$

(this ensures that the flow from bin $$p_{i}$$ to bin $$q_{j}$$ is positive)

$$\Sigma^{n}_{j=1} f_{ij} \leq w_{pi}$$

(this ensures that the outward flow from bin $$p_{i}$$ is equal to the weight at bin $$p_{i}$$)

$$\Sigma^{m}_{i=1} f_{ij} \leq w_{qj}$$

(this ensures that the inward flow to bin $$q_{j}$$ is equal to the capacity at bin $$q_{j}$$)

$$\Sigma^{m}_{i=1} \Sigma^{n}_{j=1} f_{ij} =\text{min} \left( \Sigma^{m}_{i=1} w_{pi}, \Sigma^{n}_{j=1} w_{qj} \right) $$

(this ensures that the maximum amount of supply is moved : if this was not there, then one solution could be that there is no flow at all!)

Lastly, the EMD is computed as

$$ \text{EMD} (P, Q) = \frac{\Sigma^{m}_{i=1} \Sigma^{n}_{j=1} d_{ij}f_{ij}}{\Sigma^{m}_{i=1} \Sigma^{n}_{j=1} f_{ij}} $$

Of course, it is important that the EMD can be computed efficiently especially if the (1-D) histogram has a large number of bins. An example of a fast computation strategy is demonstrated in [Cuturi](https://arxiv.org/abs/1306.0895) where the author shows that by adding a regularization loss term (called the sinkhorn loss), the problem of minimization reduces from a *convex* problem to a *strictly convex* problem which is solveable several orders of magnitude faster. A nice overview of the same is available on [this](https://michielstock.github.io/OptimalTransport/) blog post!


