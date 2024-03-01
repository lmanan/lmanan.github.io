---
layout: post
title: Human Pose as Compositional Tokens 
categories: Computer Vision
tags:
mathjax: true
comments: true
---


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

>"We represent raw pose as $G \in R^{K \times D}$ where $K$ is the number of body joints and $D$ is the dimension of each joint (D =2 for 2D pose, 3 for 3D pose)".




