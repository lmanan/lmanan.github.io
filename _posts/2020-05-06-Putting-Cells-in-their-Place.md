---
layout: post
title: Putting Cells in their place
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-05-06/cells.png" alt=""/><figcaption>
[Source: Faridani and Sandberg, 2015. Schematic illustration of spatial reconstruction of single cell gene expression. (a) Enzymatic dissociation of a tissue into a single-cell suspension, followed by single-cell RNA sequencing and clustering of cellular expression profiles (b) A database of *in situ* hybridization images are computationally aligned onto a reference map, analyzed and used to generate regional expression profiles (c) Methods are developed to computationally project the RNA-seq data from individual cells onto the reference map and to compute the degree of confidence in mapping. ]</figcaption></figure></p>

(From [Satija et al](https://www.nature.com/articles/nbt.3192), 2015)
>"First, single-cell RNA-seq measurements are confounded by technical noise, particularly false negatives and measurement errors for low-copy transcripts. As only a few landmark genes characterize each region of the spatial map, erroneous measurements for these genes in a given cell could interfere with its proper localization. To address this, Seurat leverages the fact that RNA-seq measures multiple genes that are co-regulated with the landmark genes and uses these genes to impute the values of the landmark genes. Specifically, Seurat uses the expression levels of all highly variable genes in the RNA-seq data set and an L1-constrained, LASSO (least absolute shrinkage and selection operator technique to construct separate models of gene expression for each of the landmark genes. In this way, expression measurements across many correlated genes ameliorate stochastic
noise in individual measurements."

Satija and colleagues employ *Seurat* to map 851 single cells arising from the late blastula stage of dissasociated Zebra fish embryos to a reference atlas and thus assign these cells a spatial location within the embryo. *Seurat* uses a statistical framework to combine cells' expression profiles with complementary in situ hybridization data for a smaller set of landmark genes. The authors achieve this by subdividing the tissue of interest into spatial domains or *bins* of a certain geometry and shape. Next, for the *landmark genes*, they assign a binary tag of *on* or *off* in each bin.

The authors claim that the single cell sequencing comes with technical noise and a higher tendency of false negatives. In order to reduce such occurences, they look at the coexpression patterns of all highly variable genes and the LASSO technique to construct separate models of gene expression for each of the landmark genes. 

