---
layout: post
title: Deriving VAE losses 
categories: Computer Vision
tags:
mathjax: true
comments: true
---

Much of the text below is inspired from [Odaibo](https://arxiv.org/pdf/1907.08956.pdf).

The encoder yields an approximate posterior distribution $$ q(z \vert x) $$ parameterized on a neural network by weights collectively denoted by $$\theta$$. hence we refer to the encoder properly as $$q_{\theta} (z \vert x) $$.

Similarly the decoder portion yields a likelihood distribution parameterized on a network by weights $$\phi$$ : so it is properly referred to as $$ p_{\phi} (x \vert z) $$.

The KL divergence between the approximate and the actual posterior distribution is:

$$ \text{KL} \left( q_{\theta}(z \vert x_{i}) \vert \vert p(z \vert x_{i}) \right) = \int q_{\theta}(z \vert x_{i}) \log \frac{q_{\theta}(z \vert x_{i}) }{ p(z \vert x_{i})} \text{d}z \geq 0 $$


Applying Bayes' Law
 
$$ = - \int q_{\theta}(z \vert x_{i}) \log \frac{p(z \vert x_{i}) }{ q_{\theta}(z \vert x_{i})} \text{d}z \geq 0 $$

$$  = - \int q_{\theta}(z \vert x_{i}) \log \frac{p_{\phi} (x_{i} \vert z)p(z)}{q_{\theta} (z \vert x_{i})p(x_{i})} $$

This can be broken down:

$$ \text{KL} \left( q_{\theta}(z \vert x_{i}) \vert \vert p(z \vert x_{i}) \right) = - \int q_{\theta} (z \vert x_{i} ) \log \frac{p_{\phi} (x_{i} \vert z) p(z)}{q_{\theta} (z \vert x_{i}} \mathrm{d}z + \int q_{\theta} (z \vert x_{i}) \log  p(x_{i}) \geq 0 $$

Since $$\log p(x_{i})$$ is a constant and since $$\int q_{\theta} (z \vert x_{i}) $$ integrates to one, hence


$$ \text{KL} \left( q_{\theta}(z \vert x_{i}) \vert \vert p(z \vert x_{i}) \right) = - \int q_{\theta} (z \vert x_{i} ) \log  \frac{p_{\phi} (x_{i} \vert z) p(z)}{q_{\theta} (z \vert x_{i}} \mathrm{d}z + \log p(x_{i}) \geq 0 $$


This implies:

$$ \log p(x_{i}) \geq \int q_{\theta} (z  \vert x_{i}) \left[  \log p_{\phi} (x_{i} \vert z) + \log p(z) - \log q_{\theta} (z \vert x_{i})  \right] \mathrm{d}z $$

Recognizing that the right hand side is an expectation, we write

$$ \log p(x_{i}) \geq E_{\sim q_{\theta} (z  \vert x_{i})} \left[  \log p_{\phi} (x_{i} \vert z) + \log p(z) - \log q_{\theta} (z \vert x_{i})  \right] $$

$$ \log p(x_{i}) \geq E_{\sim q_{\theta} (z  \vert x_{i})} \left[  \log p_{\phi} (x_{i}, z) - \log q_{\theta} (z \vert x_{i})  \right]  $$

Not sure where the two equations above lead up to, but going back a few more steps we notice that:

$$ \log p(x_{i}) \geq \int q_{\theta} (z  \vert x_{i}) \left[  \log p_{\phi} (x_{i} \vert z) + \log p(z) - \log q_{\theta} (z \vert x_{i})  \right] \mathrm{d}z $$

Expanding:

$$ \log p(x_{i}) \geq \int q_{\theta} (z  \vert x_{i}) \log \frac{p(z)}{q_{\theta}(z \vert x_{i})} \mathrm{d}z + \int q_{\theta} (z \vert x_{i}) \log p_{\phi} (x_{i} \vert z) \mathrm{d}z $$

$$ \log p(x_{i}) \geq - D_{KL} ( q_{\theta}(z \vert x_{i}) \vert \vert p(z)) + E_{\sim q_{\theta} (z \vert x_{i})} \log p_{\phi} (x_{i} \vert z) $$


The first term on the right hand side of the equation above is referred to as *Evidence Lower Bound* (ELBO). Maximising it leads to maximising the likelihood. The second term is referred to as reconstruction term because it is a measure of the reconstructed data.

We can obtain a closed form solution of the loss above if we choose a gaussian representation of the prior $$ p(z)$$ and also the approximate posterior $$ q_{\theta} (z \vert x_{i}) $$.


## Closed Form VAE

If we choose the prior as :

$$ p(z) = \frac{1}{\sqrt{2 \pi \sigma_{p}^{2}}} \exp{- \frac{(x-\mu_{p})^{2}}{2 \sigma_{p}^{2}}} $$


And the approximate posterior as :

$$ q_{\theta} (z \vert x_{i}) = \frac{1}{\sqrt{2 \pi \sigma_{q}^{2}}} \exp{- \frac{(x-\mu_{q})^{2}}{2 \sigma_{q}^{2}}} $$

then the ELBO becomes:

$$ - D_{KL} ( q_{\theta}(z \vert x_{i}) \vert \vert p(z)) = 
\int \frac{1}{\sqrt{2 \pi \sigma_{q}^{2}}} \exp{- \frac{(x-\mu_{q})^{2}}{2 \sigma_{q}^{2}}} \log \left( \frac{\frac{1}{\sqrt{2 \pi \sigma_{q}^{2}}} \exp{- \frac{(x-\mu_{q})^{2}}{2 \sigma_{q}^{2}}}}{\frac{1}{\sqrt{2 \pi \sigma_{p}^{2}}} \exp{- \frac{(x-\mu_{p})^{2}}{2 \sigma_{p}^{2}}}} \right) \mathrm{d}z $$


This simplifies to become:

$$ - D_{KL} ( q_{\theta}(z \vert x_{i}) \vert \vert p(z)) = \frac{1}{\sqrt{2 \pi \sigma_{q}^{2}}} \int \exp{- \frac{(x-\mu_{q})^{2}}{2 \sigma_{q}^{2}}}  

\left[
 \log (\frac{\sigma_{q}}{\sigma_{p}}) - \frac{(x- \mu_{p})^{2}}{2 \sigma_{p}^{2}} + \frac{(x- \mu_{q})^{2}}{2 \sigma_{q}^{2}} 
\right]

\mathrm{d}z $$

Now if this integral has a clean analytical solution, then our problem is solved! Note that in the four equations above, x is $$ \sim $$ z and is not the same as $$x_{i}$$

 
