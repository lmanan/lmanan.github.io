---
layout: post
title: Message Parsing for Neural Networks
categories: Computer Vision
tags:
mathjax: true
comments: true
---

[Gilmer and colleagues](https://arxiv.org/pdf/1704.01212.pdf) reformulate existing models into a single common framework, call it Message Parsing Neural Networks and use it for predicting specific molecular properties. 

The authors use their setup to predict 13 properties for each molecule, which traditionally are computationally approximated by an expensive quantum mechanical simulation method (DFT). This yields 13 regression tasks. While measuring perfoirmance of the model, the authors investigate two important benchmark error levels. The first is the average error of the DFT approximation to nature or the *DFT error* and the second is *chemical accuracy*. Through their approach, the authors are able to predict the DFT calculation of all but two targets to within chemical accuracy.


The authors begin by summarizing the other methods that use a variant of message parsing. According to them, there are atleast 8 such notable works. The convention followed by the authors is that in an undirected graph, node features are denoted as $$x_{v}$$ and edge features are denoted as $$e_{vw}$$. The forward phase has two steps: a message passing phase and a readout phase.  The message passing phase runs for $$T$$ iterations and is defined in terms of message functions $$M_{t}$$ and update functions $$U_{t}$$.

$$m_{v}^{t+1} =\sum_{w} M_{t}(h_{v}^{t}, h_{w}^{t}, e_{vw})$$

$$h_{v}^{t+1} = U_{t} (h_{v}^{t}, m_{v}^{t+1})$$

The readout phase computes a feature vector for the whole graph using readout fuction $$R$$

$$\hat{y} = R(\{ h_{v}^{T} | v \in G \})$$

Note that $$M_{t}, U_{t} \& R$$ are all learned differentiable functions. 
