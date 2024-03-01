---
layout: post
title: Human Pose as Compositional Tokens 
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Geng et al, 2023](https://arxiv.org/pdf/2303.11638.pdf))
>" Human pose is typically represented by a coordinate vector of body joints or their heat map embeddings. While easy for data processing, unrealistic pose estimates are admitted due to the lack of dependency modeling between the body joints."

If instead of learning not just the heat map embeddings, you also predict a pair wise distance map between all joints, the dependency modeling is taken care of, no?

>"In this paper, we present a structured representation, named Pose as Compositional Tokens to explore the joint dependency."

What do they mean by a structured representation?

> "It represents a pose by $M$ discrete tokens with each characterising a sub structure with several interdependent joints."

How is the sub structure determined - i.e. how does one know which joints would be grouped within which substructure?

>"In this work, we hope to learn the dependency between the joints earlier in the representation stage without any assumptions. Our initial idea is to learn a set of prototype poses that are realistic, and represent every pose by the nearest prototype."

Ah, very cool! Can be applied to biological data where the animal shows only a few conformations.
Do we need to know the number of discrete conformations? Are these prototypical poses discrete?
Is the Stage 1 unsupervised, by the way? What is the information being input in the auto-encoding step? Just the graph with nodes? What is the feature on the node and edges? Is the connectivity provided? How is the graph processed by a VAE?

>"We represent raw pose as $G \in \mathcal{R}^{K \times D}$ where $K$ is the number of body joints and $D$ is the dimension of each joint (D =  2 for 2D pose, 3 for 3D pose)".

>"Note that the representation has a lot of redundancy because different tokens may have overlapping joints. The redundancy makes it robust to occlusions of different joints."

>"Similar to 85, we define a latent embedding space by a code book $C = (c_{1}, ..., C_{v})^{T} \in \mathcal{R}^{V \times N}$ where $V$ is the number of codebook entries."

So $V$ and $M$ aren't the same? Tokens have dimensionality $\mathcal{R}^(M \times H)$. While codebook has dimensionality $\mathcal{R}^{V \times N}$. Hmm. What is $N$?

>"We try to explain why PCT learns tokens that correspond to meaningful sub structure of poses. At one extreme, if each token corresponds to a single joint, then we need $w \times h$ codebook entries to achieve a small quantization error. But we only use 1024 entries in our experiments which is much smaller. This drives the model to learn larger structures than individual joints to improve the efficiency of the codebook. At another extreme, if we let a token correspond to an intact pose, then we only need one token instead of M tokens. But in the worst case, it requires $(wh)^{K}$ codebook entries with a small error."

>"As shown in Fig3, we simply predict the categories of the M tokens, which are fed to the decoder to recover the pose."

Is GT available for these M tokens? How is the categories specified to supervise the training?


