---
layout: post
title: Graduated Assignment for Multi-Graph Matching
categories: Biology
tags:
mathjax: true
comments: true
---


(From [Wang et al, 2020](https://proceedings.neurips.cc/paper/2020/file/e6384711491713d29bc63fc5eeb5ba4f-Paper.pdf))
>"... current supervised deep graph matching requires costly annotation on large-scaled training data, which restricts the real-world application of modern deep graph matching methods. Motivated by the fact that MGM will usually result in better matching results compared to pairwise matching, it is appealing to devise an unsupervised deep multi-graph matching learning algorithm by utilizing multi-graph matching information as the pseudo label for pairwise matchings."


Wang et al, 2020 address the joint problem of multi-graph matching (MGM) and clustering. The authors suggest that current learning-based approaches often consider *pair-wise* graph matching (and not multi-graph matching) and require the annotation of corresponding keypoints between the source and target graph to serve as supervision, the preparation of which is costly. 

Additionally, among the works which address multi graph matching specifically, one general strategy is to consider one of the graphs as a prototype graph and to match the remaining graphs to this prototype graph. The choice of the first, anchor graph could bias the final MGM result. The authors' approach is free from this choice. 

>"Specifically, [14] proposes a graduated assignment MGM algorithm, yet a ‘prototype’ graph is required as the anchor (in their paper they use the first graph as the ‘prototype’). This means they assume the bijection between each pair of graphs which is hard to satisfy in practice. In contrast, our proposed method is fully decentralized free from such a constraint, and it can therefore handle the setting of partial matching, i.e. only part of the nodes in each graph can find their correspondence from the other graphs. This setting is more practical and challenging."

The authors choose the Koopman-Beckman's QAP (KP-QAP) to maximize while solving the MGM. For a pairwise graph matching between two graphs $G_{i}$ and $G_{j}$, the KB-QAP is:

$$
\text{max}_{X_{ij}} \lambda \text{tr} \left( X_{ij}^{T} A_{i} X_{ij} A_{j} \right)  + \text{tr} \left( X_{ij}^{T} W_{ij} \right)
$$

such that the sum of any row or any column in $X_{ij}$ is 1 and the entries of $X_{ij}$ can be either $0$ or $1$. 

Here, $A_{i}$ and $A_{j}$ are weighted adjacency matrices. The authors obtain this by first calculating the euclidean distance between all keypoints in a graph. Then for any two keypoints $a$ and $b$, 

$$
A_{i}[a, b] = \text{exp} \left( \frac{-l_{ab}^{2}}{\hat{l}^{2}} \right)
$$

where $\hat{l}$ is the median value for all pairwise distances in a given graph.

An interesting strategy used by the authors is to express $X_{ij}$ as $U_{i}U_{j}^{T}$ where $U_{i}$ has dimensionality $N_{i} \times d$ where $d$ is the size of the universe of nodes and $N_{i}$ is the number of nodes in the graph $G_{i}$. In such graph matching problems, typically, the ideal number of nodes (or keypoints) in the source and target graphs is known beforehand, which allows setting $d$ appropriately. (I think this formulation won't work if one were to use this in a tracking context where the number of nodes (cells) per time point are not known beforehand, thus making it difficult to set $d$ suitably.)


If we ignore the mapping to the universe of nodes for now, and just consider a Taylor expansion of the KB-QAP objective $Q$, keeping only the first derivative terms, then my guess is that:

$$ 
Q \simeq Q_{X=X_{0}} + \frac{\partial Q}{\partial X}|_{X=X_{0}} \left( X-X_{0}\right)
$$

Maximizing $Q$ would then be akin to starting from some initial condition $X=X_{0}$ and maximizing (in each step of the iteration procedure until convergence)

$$
\frac{\partial Q}{\partial X}|_{X=X_{0}} \times X 
$$ 

which turns out to be a linear assignment problem.

Also, my guess (still have to verify the derivatives) would be that this statement above (which needs to be maximized in each step of the iteration procedure) is equal to 

$$
\text{tr} \left[ \left(2 \lambda A_{i} X_{ij}^{0} A_{j} + W_{ij} \right)^{T} \times X_{ij} \right]
$$



