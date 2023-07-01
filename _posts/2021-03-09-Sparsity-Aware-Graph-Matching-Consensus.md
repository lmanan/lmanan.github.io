---
layout: post
title: Graph Matching Consensus
categories: Computer Vision
tags:
mathjax: true
comments: true
---

[Fey et al, 2020](https://openreview.net/pdf?id=HyeJf1HKvS) state that the problem of graph matching has typically been addressed through three approaches: by determining a graph edit distance between the two graphs being considered, by solving the maximum common sub graph problem or through solving the quadratic assignment problem. All these approaches are NP hard which implies that solving them to optimality on large graphs would be intractable.

Recently, many neural architectures were proposed to solve the task of graph matching. Notable amongst these was [Zanfir and Sminchisescu, 2018](https://openaccess.thecvf.com/content_cvpr_2018/papers/Zanfir_Deep_Learning_of_CVPR_2018_paper.pdf) where the authors learn unary and pairwise features which would provide a doubly stochastic confidence map between all nodes from graph 1 and all nodes from graph 2. (Here, doubly stochastic implies that the sum of entries along any row equals the number of columns. Also, sum of entries along any column equals the number of rows). This non-square confidence map is further processed to provide a pixel offset loss (referred to as the "displacement loss" in the publication). As GT during training, the pixel offset vectors between corresponding nodes are available to the network. Updating this approach, [Wang et al, 2019](https://openaccess.thecvf.com/content_ICCV_2019/papers/Wang_Learning_Combinatorial_Embedding_Networks_for_Deep_Graph_Matching_ICCV_2019_paper.pdf) proposed a different neural net architecture and a permutation loss based on sinkhorn iterations. Here, the authors claimed that instead of providing GT pixel offset distances, one can directly make available the GT permutation matrix for supervision, and utilize this information for end-to-end training. Fey et al claim that these approaches might be prone to matching neighborhoods between graphs inconsistently by taking only localized node embeddings into account. Here the authors propose using a second stage  that *fine-tunes* the matching prediction provided by the first stage of the end-to-end trained neural network. To me, this additional approach is reminiscent of employing ICP from a good starting condition (*good* implying that moving/source graph is already somewhat registered to the fixed/target graph. One difference to ICP of course is that the weights of the second neural net are also learnt unlike in ICP where there are no trainable parameters). Furthermore, the authors state that their appproach is more likely to scale well to graphs with many nodes. 

This key consensus idea as proposed by Fey et al was initially suggested in [Rocco et al, 2018](https://www.di.ens.fr/willow/research/ncnet/). Generally speaking, if a node a from graph 1 is assigned to node M from graph 2, then it is desired that another node b which falls in the neighborhood of a should be assigned to a node N which falls in the neighborhood of M. This is achieved by maximizing the following objective:

$$
\sum_{i, i^{'} \in V_{s}\\ j, j^{'} \in V_{t}} A^{s}_{i, i^{'}}  A^{t}_{j, j^{'}} S_{i,j} S_{i^{'}, j^{'}}
$$

Here, $s$ denotes source graph and $t$ denotes target graph. $i^{'}$ denotes the index of node in the source graph which is in the neighborhood of another node with index $i$ in the source graph. Similarly, $j^{'}$ denotes the index of node in the target graph which is in the neighborhood of another node with index $j$in the target graph. $A$ implies the adjacency matrix generated from a graph.

I covered Rocco et al, 2018 earlier [here](https://mlbyml.github.io/Neighbourhood-Consensus-Networks/), but the gist of the paper is that given two 2d images which need to be matched, one can obtain a 4d correlation map  using learnt features from a previously pretrained model. Then the authors employ 4d convolutions on this 4d correlation map to amplify and suppress matches based on supporting evidence in their neighborhoods. While these 4d convolutions provide semi-local similarity, there is no global constraint on matches, for which the authors suggest a simple filtering idea to ensure that matched features are required to be mutual nearest neighbors. They apply a soft gating to the filtered 4d convolution map $c_{ijkl}$ as follows:

$$
\hat{c}= r_{ijkl}^{A} r_{ijkl}^{B} c_{ijkl} 
$$

$$
r_{ijkl}^{A} = \frac{c_{ijkl}}{\text{max}_{ab} c_{abkl}}
$$

$$
r_{ijkl}^{B} = \frac{c_{ijkl}}{\text{max}_{cd}c_{ijcd}}
$$

Here $A$ and $B$ are the source and target image. This approach essentially downweights scores of matches that are not mutual nearest neighbors. 

In Fey et al, the authors address two use cases - supervised where permutation matrices are available for all nodes in the two graphs and semi supervised where permutation matrices are available for only a subset of nodes in the two graphs.

The architecture of the first stage of the network is a GNN, which is used to obtain localized, permutation invariant node embeddings. The loss is the sinkhorn loss which essentially minimized  negative log likelihood of correct correspondence scores.

$$
L^{\text{stage-1}} = - \sum_{i \in V_{s}} \text{log} (S_{i, \pi_{gt}(i)}i) 
$$ 


Here, $S$ is the bistochastic matrix obtained through sinkhorn iterations on the product of the rich contextual features generated by the network. i.e.

$$
S = \text{sinkhorn}(\hat{S})
$$

$$
\hat{S} = H_{s}H_{t}^{T}
$$ 






