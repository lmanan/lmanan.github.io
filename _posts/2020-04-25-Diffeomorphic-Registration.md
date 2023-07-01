---
layout: post
title: Diffeomorphisms
categories: Computer Vision
tags:
mathjax: true
comments: true
---
(From [Ashburner](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.474.1033&rep=rep1&type=pdf), 2007)
>"The large-deformation or diffeomorphic setting is a much more elegant framework. A diffeomorphism is a globally one-to-one(objective) smooth and continuous mapping with derivatives that are invertible (i.e. nonzero Jacobian determinant). If the mapping is not diffeomorphic, then topology is not necessarily preserved. A key element of a diffeomorphic setting is that it enforces consistency under compositions of the deformations."

Ashburner comments that many registration approaches employ a small deformation model $\phi(x) = x + u(x) $. Here these models parameterize a displacement field $u$ which is simply added to the identity transform $u$. Assuming Taylor expansion, the inverse transformation can be approximated by subtracting the displacement $ \phi^{-1}(x) = x - u(x)$. But this model would fail for larger deformations, and often one would notice that the composition of the forward and inverse deformations do not result in an identity transform. Hence, this elucidates the importance of using diffeomorphic deformations, where the compositions of the forward and inverse transform results in an identity transform.

The evolution of one such diffeomorphism is expressed through the following differential equation:

$$ \frac{d \phi}{dt} = u \phi$$

In this publication, Ashburner employs a constant velocity field $u$. Next by using Euler's forward integration method, the differential equation is approximated as follows:

$$ \phi_{t+h} = \phi_{t} + hu \phi_{t}$$

Furthermore if the number of timesteps is chosen as 8, then one could use a recursive relation as given below:

$$ \phi_{\frac{1}{8}} = x + \frac{u}{8}$$

$$ \phi_{\frac{1}{4}} = \phi_{\frac{1}{8}} \circ \phi_{\frac{1}{8}} $$

$$ ... $$

$$ \phi_{1} = \phi_{\frac{1}{2}} \circ \phi_{\frac{1}{2}}$$

(From [Dalca et al](https://arxiv.org/abs/1805.04605), 2018)
>"Further-more, learning-based registration tools have not been derived from a
probabilistic framework that can offer uncertainty estimates."

Dalca and colleagues include this *Scaling and Squaring Strategy* in their unsupervised, CNN-based, probabilistic framework built around the idea of guaranteeing a topology-preserving registration and offering uncertainty estimates.

In their publication, they model the prior probability of the latent variable $z$ (which represents the stationary velocity field) as:

$$ p(z) = N (z; 0, \Sigma_{z}) $$

Let $x$ be a noisy observation of the warped image $y$

$$ p(x \vert z; y) = N(x; y \circ \phi_{z}, \sigma^{2} I) $$

Estimating the posterior $$ p(Z \vert x;y) $$ is intractable. So instead we introduce an approximate posterior probability $$ q_{\psi} (z \vert x; y) $$ parameterized by $$ \psi $$ and minimize the distance between the two distributions:

$$ \text{min}_{\psi} \text{KL} [q_{\psi}(z|x; y)  \vert \vert p(z \vert x; y) ] $$
