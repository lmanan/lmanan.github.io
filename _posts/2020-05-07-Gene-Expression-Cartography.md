---
layout: post
title: Gene Expression Cartography
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-05-07/novosparc.png" alt=""/><figcaption>
[Source: Nitzan et al, 2019. A matrix that contains single-cell transcriptome profiles sequenced from disassociated cells is the main input for novoSpaRc. The output is a virtual tissue of a chosen shape ]</figcaption></figure></p>



(From [Nitzan et al](https://www.nature.com/articles/s41586-019-1773-3), 2019)
>"Here, we specifically explore the assumption that cells that are physically close tend to share similar transcription profiles, and vice versa. Biologically, this phenotype can result from multiple mechanisms, such as gradients of oxygen, morphogens and nutrients, the trajectory of cell development and communication between neighbouring cells. We stress that this is an assumption about overall gene expression across the entire tissue—not about individual genes and not about all cells that are physically close. We show that, on average, the distance
between cells in expression space increases with their physical distance, for diverse tissues in mature organisms or whole embryos in early development."

Nitzan and colleagues address the scenario when a reference atlas is not available and thus a set of marker genes with spatial locations is not at hand. The authors employ a general principle that cells which are proximate spatially are also near in the expression space.

### Experiment One
As a proof of concept, the authors chose certain tissues (mammalian intenstinal epithelium and liver lobules) which have inherent symmetries and can be perceived as one dimensional. For such tissues, they additionally had marker gene information which allows segregating the tissue in distinct zones (7 for mammalian intenstinal epithelium and 9 for liver). They noticed that upon reconstruction, the pairwise distance between cells in expression space increased monotonically with the pairwise distance in the physical space. Additionally, the authors mention that if one were to simply employ the top 100 variable genes then the pearson correlation is >0.94 for both setups. This technique is also robust to the number of embedded zones.  

<p><figure><img src="/images/2020-05-07/exp1.png" alt=""/><figcaption>
[Source: Nitzan et al, 2019. Reconstruction scheme for the mammalian intestinal epithelium (a) and liver lobules (e). b, f: Demonstration of the monotonic relationship between cellular pairwise distances in expression and physical space. c, g: Novosparc infers the spatial context of zones/layers with high accuracy. d, h: Predicted expression pattern of genes in layers/zones]</figcaption></figure></p>



### Experiment Two
Next, the authors focussed on reconstructing the well known Drosophila embryo in stage 5 of development with 6000 cells. The expression levels of 84 transcription factors were quantitatively registered using fISH for each of the cells. The authors performed a virtual scRNA-seq experiment where they disassociated these 6000 cells and then attempted to reconstruct the original expression patterns across the tissue both de novo and using marker genes.

An interesting observation is that since there is no anchor point in the virtual, predicted spatial map, it would in most cases is similar up to global transformations. Consequently the resultant map might be shifted or flipped relative tothe expected ones. 

<p><figure><img src="/images/2020-05-07/exp2.png" alt=""/><figcaption>
[Source: Nitzan et al, 2019. a. FISH data are used to create virtual scRNA seq data b. Pairwise cellular distances in expression space increase monotonically with pairwise distances in distance space c. Novospark reconstructs the spatial map with just one marker gene. The quality improves with more genes but saturates at two marker genes d. Visualization of results for four marker genes e. The original locations of three cells is compared to their preicted respective locationby Novospark ]</figcaption></figure></p>


