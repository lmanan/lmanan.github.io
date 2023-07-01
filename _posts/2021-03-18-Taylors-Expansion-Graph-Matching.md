---
layout: post
title: Taylor's Expansion and Graph Matching 
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Gold and Rangarajan, 1996](https://ieeexplore.ieee.org/document/491619))
>"Our graduated assignment method falls under the rubric of nonlinear optimization. Like relaxation labeling, it does not search a state-space and has a low order computational complexity O(lm). It differs from relaxation labeling in two ways. The softassign, incorporating a method discovered by Sinkhorn is employed here to satisfy two-way constraints. ... Second, a continuation method - graduated nonconvexity - is used in an effort to avoid poor local minima, with a parameter controlling the convexity."



[Gold and Rangarajan, 1996](https://ieeexplore.ieee.org/document/491619) provide an elegant approach to graph matching using the Taylor's Expansion. Their approach scales with $O(lm)$ where $l$ and $m$ are the number of edges in the two graphs being considered, allows for matching graphs with unequal number of nodes. 

They address the problem of graph matching as the problem of minimizing the following objective function :


$$
E_{wg} (M) = -\frac{1}{2} \sum_{a=1}^{A} \sum_{i=1}^{I} \sum_{b=1}^{A} \sum_{j=1}^{I} M_{ai}M_{bj}C_{aibj}
$$

Here, $M$ is the match matrix which needs to be found s.t its terms can only take values 0 and 1 i.e. $M_{ai} \in \{0,1\}$.

Also, we have the additional constraint that:

$$
\sum_{i=1}^{I} M_{ai} \leq 1
$$

and 

$$ 
\sum_{a=1}^{A} M_{ai} \leq 1 
$$


Finally, 

$$
C_{aibj} = 0 \text{ if either $G_{ab}$ or $G_{ij}$ is null }
$$

and 

$$
C_{aibj} = c(G_{ab}, G_{ij})
$$

such costs could be for example the difference of the length of edges or the difference of the orientation of edges or a weighted sum of both!

The authors motivate their method by first elucidating a simple approach for converting a discrete permutation matrix (a permutation matrix is a square zero-one matrix such that the sum of rows and columns is one) to a continuous permutationmatrix. Here they introduce a hyper parameter $\beta$ which can be used along with the softmax function and sinkhorn approach to ensure that the sum of the rows and columns is one. The general procedure is:

* Consider the current terms of M (which are discrete 0s and 1s)
* Replace each term of M by $\text{exp}(\beta M)$
* Normalize along all rows such that sum is one
* Normalize along all columns such that sum is one


Hereafter comes a simple approach from the authors. By leveraging the first-order Taylor's expansion, one could reduce the quadratic dependence of $E$ on $M 
$ to a linear dependence. This strategy is not much unlike the gradient descent followed while training neural networks. Essentially what the authors say is:

$$
E (M) = E_{0} +  \frac{\partial E}{\partial M}|_{M=M_{0}} (M-M_{0})
$$

Now in this formulation, the first term on the RHS is a constant. Hence, the only way to minimize $E(M)$ is to minimize the second term on the RHS. 

The key thing to keep in mind is that large $\beta$ would approximate a discrete solution, so while using a large $\beta$ the difference between the discrete and continuous form of $M$ is small, and vice-versa. In that regard, one should go from a low to a high $\beta$ which is somewhat similar to going from a high learning rate to a low learning rate while scheduling the learning rate profile during training a NN. 

I find it a bit strange that the $M$ is set equal to $\text{exp}(\beta Q)$ in each iteration, without including the $M$ from the previous iteration in the formulation. (Usually one would combine the estimate from the previous iteration and the gradient calculated at the previous iteration). So, I would rather do it like this:

* Start with a $M_{0, d}$ where 0 implies permutation matrix coming from some algorithm (say shape context matching) and $d$ implies discrete
* Convert it to its bistochastic form by using some $\beta_{0}$ i.e. now we have $M_{0, c}$ where $c$ implies continuous
* Find gradient of $E$ wrt $M_{0,c}$ i.e. $Q$
* New $M_{1} = M_{0,c} - \alpha*Q$ 
* Convert $M_{1}$ to its bistochastic form with a raised $\beta$ i.e. now we have $M_{1,c}$
* Repeat until $E$ converges

The only issue I see above is there are two hyper parameters - $\alpha$ and $\beta$. Also, not guaranteed that this would reach a better minimum.
One way to fix this could be to set $\beta$ to a large value say $1e3$ and only have scheduling on $\alpha$.


In the [Deep Graph Matching Consensus](https://arxiv.org/pdf/2001.09621.pdf) work, the authors pointed out the analogy of their approach to Graduated Assignment. On Page 5 of the pdf, it seemed to me that that the update strategy in GA for $M$ is actually to do soft-assign $ Q M_{\text{previous}}$. This makes more sense! 

The authors of DGMC further point out that $Q$ can be seen as cosine similarity between the source and target node embeddings while including their corresponding adjacency matrices. 







