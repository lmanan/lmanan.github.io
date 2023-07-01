---
layout: post
title: Generative Modeling using Nonequilibrium Thermodynamics 
categories: Biology
tags:
mathjax: true
comments: true
---


(From [Sohl-Dickstein et al, 2015](https://proceedings.mlr.press/v37/sohl-dickstein15.html))
>"Historically, probabilistic models suffer from a trade-off between two contrasting objectives: tractability and flexibility. Models that are tractable can be analytically evaluated and easily fit to data. However, these models are unable to aptly describe structure in rich datasets. On the other hand, models that are flexible can be molded to fit structure in arbitrary data ... Evaluating, training, or drawing samples from such flexible models typically requires a very expensive Monte Carlo process".

Sohl-Dickstein et al, 2015 build a generative Markov chain that converts a known distribution such as Gaussian into a target distribution using a diffusion process. 

>"Learning in this framework involves estimating small perturbations to a diffusion process. Estimating small perturbations is more tractable than explicitly describing the full distribution with a single, non-analytically-normalizable potential function."

The forward or diffusion process is fixed to a Markov Chain that gradually adds gaussian noise to data according to a variance scheduler:

$$
q(\textbf{x}_{1:T}|\textbf{x}_{0}) := \prod_{t=1}^{T}q(\textbf{x}_{t}|\textbf{x}_{t-1})
$$

and 

$$
q(\textbf{x}_{t}|\textbf{x}_{t-1}) := \mathcal{N}(\textbf{x}_{t}; \sqrt{1-\beta_{t}} \textbf{x}_{t-1}, \beta_{t} \textbf{I})
$$

The reverse or generative process is modeled as:

$$
p(\textbf{x}_{T}, \textbf{x}_{T-1}, \ldots, \textbf{x}_{0}) := p(\textbf{x}_{T}) \prod_{t=1}^{T} p_{\theta} (\textbf{x}_{t-1}|\textbf{x}_{t})
$$

Here,

$$
p_{\theta} (\textbf{x}_{t-1}|\textbf{x}_{t}) := \mathcal{N}(\mathbf{x}_{t-1}; \mathbf{\mu}_{\theta}(\mathbf{x}_{t}, t), \mathbf{\Sigma}_{\theta}(\mathbf{x}_{t}, t))
$$


In [Graikos et al, 2022](https://arxiv.org/pdf/2206.09012.pdf), the authors argue that the immense effective depth of DDPMs limits their use as off the shelf modules where one model may serve as a prior for another conditional model. The authors also say that in existing work on plug and play models, there is an aspect of fine tuning, which is not needed in their work. Lastly, the authors claim that their strategy provides a more robust solution to domain adaptation.
