---
layout: post
title: CellTypeGraph
categories: Computer Science
tags:
mathjax: true
comments: true
---

(From [Cerrone et al, 2022](https://openaccess.thecvf.com/content/CVPR2022/papers/Cerrone_CellTypeGraph_A_New_Geometric_Computer_Vision_Benchmark_CVPR_2022_paper.pdf))
>"Understanding morphogenesis, the generation of form, remains a major challenge in biology. It requires a detailed quantitative description of the molecular and cellular processes of the underlying mechanism."

Cerrone et al, 2022 state that for understanding the morphology of cells, one requires knowing the underlying molecular and cellular processes. The authors comment that instance segmentation has been very successful at capturing detailed 3D morphologies of plant cells recently. However, automatic identification of which tissues, these cells are part of, continues to be a major problem in the field.

The authors then comment about the analogous domain of medical image analysis, where different regions of the scans carry distinctive textures which enables application of deep learning based pixel-level classification techniques. The authors say that for 3D images of plant cells, the cells lack any distinctive textures which would make applying a similar pipeline as used in medical image analysis, untenable. 

>"To succeed, any approach requires informative input features, and indeed we show that a cell adjacency graph with cell-level features is a powerful representation".

To solve the above-mentioned quandary, the authors propose considering the segmented cells as nodes of a graph, and relying on geometric features such as cell adjacencies in order to classify which tissue these cells are part of. 

The plant ovules dataset which is provided by the authors, carries curated instance segmentations of all ovule cells in addition to information about their class (tissue). More specifically, $84$ specimens are imaged, which cover $9$ developmental stages. Each developmental stage carries cells from $9$ different semantic classes. Moreover, these cell types are identified based on patterns of gene expression, cell position in space and context in terms of tissue layers. 

>"The ovule cell types are imbalanced. This not only impacts the learning procedure but also requires attention when discussing results quantitatively. An effective way to account for imbalance is to evaluate the model performance for each class independently and then report as final score the average of the single class results."

The authors mentioned that since the classes are imbalanced, if one were to just ask which class cells belong to, it might lead to a skewed picture (say if $90\%$ cells belong to one tissue (class), then a model might just predict that same class for all the present cells which leads to a seemingly high $90 \%$ accuracy, but ideally we would want to account for the prediction accuracy for the remaining $8$ classes as well, somehow)

The authors require the specimens to be globally and locally aligned since these embryos are imaged in an arbitrary manner. Next, edges are drawn between any pairs of touching cells. Finally, rotation and translation invariant features such as cell volume, cell surface area  etc are extracted per node in addition to rotation and translation covariant features. The authors mention that edge features do not contribute much to the accuracy and can be skipped.

>"All scalar features are normalized to zero mean and unit variance. Furthermore, we encode categorical features as a one-hot vector and scale vector features to unit norm."

All features are normalized before being input to the model. All tested models use softmax as the final layer activation. Amongst the tested baseline methods, Deeper GCN and its edge variant (where edge features are also considered) reported the highest class average accuracy.

In the related work, the authors make an interesting remark:
>"Arguably the most popular type of dataset for node predictions is the family of citation datasets, such as Cora, CiteSeer and PubMed. Although the task is nominally identical, the learning task here is transductive and the underlying topologies are extremely different"

I am guessing that the authors wish to imply that for the citation dataset, the entire graph is available to the network, even though only a few nodes are labeled. In contrast, for the biological datasets, the framing of the task is slightly different - some training examples (images) are fully labeled and other examples are completely unlabeled and reserved for testing, hence this would be an inductive setup?




