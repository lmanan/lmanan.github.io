---
layout: post
title: Evolution of Minimum-Cost Flow Tracking Methods
categories: Computer Vision
tags:
mathjax: true
comments: true
---



[(From Padfield et al, 2009)](https://link.springer.com/content/pdf/10.1007%2F978-3-642-02498-6_31.pdf)
>"While each biological application has specific requirements, they share several core challenges. Each data set typically contains a large number of cells to be tracked. Events such as mitosis (cell division) need to be accurately captured. Furthermore, since the field of view of the microscope is limited, some cells will move in and out of the image. Limiting the potential for cytotoxicity induced by fluorescent labels is another important factor that influences the design of imaging protocols: Low concentrations of fluorescent dyes result in low signal-to-noise which makes the segmentation of the image data challenging. Also, in order to reduce photobleaching the experiments are only imaged occasionally; while some experiments maybe imaged every 3 or 5 minutes, others may only be imaged every 30 minutes. Consequently, there is often no spatial overlap of the cells between adjacent frames"


Padfield et al in 2009 implemented a variant of minimum cost-flow based optimization approach for biomedical tracking. The minimum cost-flow problem is a linear programmming problem operating on a directed graph $G = (N_{d}, A_{c})$ with the following structure:

Minimize 
$$
\sum_{(i,j) \in A_{c}} c_{ij}x_{ij} = z
$$

such that:
$$
\sum_{i \in Af(k)} x_{ki} - \sum_{j \in Bf(k)} x_{jk} = b_{k}
$$

for all $k \in N_{d}$

and that 
$$ 
l_{ij} \leq x_{ij} \leq h_{ij}
$$

$b_{k} > 0$ if k is a source node, $b_{k} <0$ is k is a sink node and $b_{k}=0$ if k is a intermediate (transshipment) node.


What Padfield and coauthors say is that the problem of objects appearing and disappearing can be handled through minimum cost-flow. But the issues of mitosis and merging are not handled by the vanilla min-cost flow implementation because one object is present in one frame and two objects are present in the next frame. To solve that, the authors introduced coupled min cost flow so that if one edge is chosen by the optimization, then the coupled edges are also chosen.  
