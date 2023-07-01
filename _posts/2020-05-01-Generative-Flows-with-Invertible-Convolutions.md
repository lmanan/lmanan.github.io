---
layout: post
title: Generative Flow with Invertible Convolutions
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-05-01/synthetic-celebrities.png" alt=""/><figcaption>
[Source: Kingma and Dhariwal, 2018. Synthetic celebrities sampled from model trained on CelebA-HQ dataset]</figcaption></figure></p>

(From [Kingma and Dhariwal](https://papers.nips.cc/paper/8224-glow-generative-flow-with-invertible-1x1-convolutions.pdf), 2018)
>"**Exact latent-variable inference and log-likelihood evaluation**: In VAEs, one is able to infer only approximately the value of the latent variables that correspond to a datapoint. GANs' have no encoder at all to infer the latents. In reversible generative models, this can be done exactly without approximation. Not only does this lead to accurate inference, it also enables optimization of the exact log-likelihood of the data, instead of a lower bound of it."

Kingma and Dhariwal describe a flow-based generative model which is capable of realistic looking large images. The authors state that flow-based model are attractive due to the tractability of the latent-variable inference (which is only approximate in VAEs) and parallelizability during training.

To launch into this, one must first understand the *change of variables* idea and how it fits in with estimating a relation between probabilities. The equations below are inspired from [this](https://www.le.ac.uk/users/dsgp1/COURSES/LEISTATS/STATSLIDE4.pdf) reference.

Let $x = g(z)$, $g$ is a *bijective* or invertible function and $x$ and $z$ are continuous random variables.

Then probabilities defined on these random variables should sum up to one, hence

$$\int p(x) \mathrm{d}x = \int p(z) \mathrm{d} z $$

which implies 

$$\int p \circ g(z) \frac{\partial x}{\partial z} \mathrm{d}z = \int p(z) \mathrm{d} z $$

Since every bijective function is a monotonic function, if we assume that $g$ is a monotonically decreasing function, would imply that $$\frac{\partial x}{\partial z} < 0$$, then knowing that $p \circ g(z) \geq 0$ and that $$\int p \circ g(z) \frac{\partial x}{\partial z} \mathrm{d}z > 0$$, implies that we must replace$$\frac{\partial x}{\partial z}$$ by $$\vert \frac{\partial x}{\partial z} \vert $$. 

Since the equality is true for all $z$ and $x$, implies that 

$$p \circ g(z) \vert \frac{\partial x}{\partial z} \vert = p(z)$$

(This induction-like proof is perhaps not complete but conveys the general idea used for expressing relations between probablilties defined over continuous variables that are themselves linked by a bijective function).

In  most flow-based generative models, the generative process is defined as:

$$ z \sim p_{\theta} (z) $$

$$ x = g_{\theta} (z) $$

The $p_{\theta} (z) $ or the *prior* is usually considered to be uniform distribution : $$p_{\theta} (z) = N(z; 0, I) $$. The function $g$ is considered to be bijective / invertible such that $$ z = f_{\theta} (x) =g^{-1}_{\theta} (x)$$.

This function $$f$$ can be composed of a sequence of transformations $$f = f_{1} \circ f_{2} \circ \ldots \circ f_{K}$$.

$$f_{1} (x) = h_{1}$$

$$f_{2} (h_{1}) = h_{2} $$

$$f_{K} (h_{K-1}) = z $$

Then by employing the change of variables from above:

$$p_{\theta} (x) = p_{\theta} (z) \vert \text{det}(\frac{\mathrm{d} z}{\mathrm{d} x}) \vert$$

$$p_{\theta} (x) = p_{\theta} (z) \vert \text{det}(\frac{\mathrm{d} z}{\mathrm{d} h_{K}}  \frac{\mathrm{d} h_{K}}{\mathrm{d} h_{K-1}} \frac{\mathrm{d} h_{K-1}}{\mathrm{d} h_{K-2}} \ldots \frac{\mathrm{d} h_{1}}{\mathrm{d} x}) \vert $$

$$p_{\theta} (x) = p_{\theta} (z) \vert \text{det}(\frac{\mathrm{d} z}{\mathrm{d} h_{K}}) \vert \vert \text{det}(\frac{\mathrm{d} h_{K}}{\mathrm{d} h_{K-1}}) \vert \vert \text{det}(\frac{\mathrm{d} h_{K-1}}{\mathrm{d} h_{K-2}}) \vert \ldots \vert \text{det}(\frac{\mathrm{d} h_{1}}{\mathrm{d} x}) \vert$$

$$ \log p_{\theta} (x) = \log p_{\theta}(z) + \sum_{i=1}^{K} \log \vert \text{det} (\frac{\mathrm{d} h_{i}}{\mathrm{d} h_{i-1}}) \vert $$



