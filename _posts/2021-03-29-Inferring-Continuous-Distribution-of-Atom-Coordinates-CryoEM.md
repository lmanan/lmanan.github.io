---
layout: post
title: Inferring a Continuous Distribution of Atom Coordinates from Cryo-EM images
categories: Computer Science
tags:
mathjax: true
comments: true
---

(From [Rosenbaum et al, 2021](https://arxiv.org/pdf/2106.14108.pdf))
>"The challenge is that we have no ground truth knowledge of the conformations or poses, and the only information in the data comes from very noisy 2D projections of the protein structure that confound conformations and poses. To tackle this, we make use of two insights: 1. We can approximate the image formation process from atom positions to images relatively well by simulating the electron microscopy process with a differentiable renderer. 2. We can formulate stereochemical constraints directly on the inferred atom positions (e.g. respecting covalent bonds)."

Rosenbaum et al state that one of the benefits of cryoEM is simplified sample preparation which enables structure determination of the target protein. Another key differentiator to other techniques is that since the sample is shock-frozen, this allows capturing the target flexible protein in *multiple* or even a continuous distribution of configurations. 

The authors state that ideally what one would like is to map the volumetric EM image to atom coordinates (previous works, such as *cryoDRGN*, have all worked in the volume space). Additionally, the authors point out that no ground truth knowledge of the configurations or poses for a given shock-frozen sample is available. The authors mention two insights which can address these challenges above - (1) that the forward model from atom coordinates to EM image is well known and (2) some stereochemical constraints can be formulated on the inferred atom positions. In their work, the authors propose a VAE setup where the latent variables are first *decoded* to the atom coordinates and next *rendered* to produce the original image which allows obtaining the reconstruction loss between the produced rendered image and the input EM image. 





