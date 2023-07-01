---
layout: post
title: Masking Autoencoders for Estimating Distributions
categories: Computer Vision
tags:
mathjax: true
comments: true
---
<p><figure><img src="/images/2020-04-30/made.png" alt=""/><figcaption>
   [Source: Germain et al, 2015. Left: Samples from a 2 hidden layer MADE. Right: Nearest neighbour in binarized MNIST]</figcaption></figure></p>

(From [Germain et al](https://arxiv.org/pdf/1502.03509.pdf), 2015)
>" ... Its main disadvantage is that the representation it (autoencoder) learns can be trivial. For instance, if the hidden layer is at least as large as the input, hidden units can each learn to “copy” a single input dimension, so as to reconstruct all inputs perfectly at the output layer. One obvious consequence of this observation is that the loss function of Equation 3 isn’t in fact a proper  log-likelihood  function. Indeed, since perfect reconstruction could be achieved, the implied data ‘distribution’ $$q(x) = \Pi_{d} \hat{x}_{d}^{x_{d}} \left( 1- \hat{x}_{d} \right)^{1 - x_{d}}$$ could be learned to be 1 for any $x$ and thus not be properly normalized ($$\sum_{x} q(x) \neq 1$$)."

Germain and colleagues in their publication, which I found quite fun to read, highlight a simple approach to adapt autoencoder neural networks, to make them estimators of distributions. Consider a training set $$ \{ x^{(t)} \}^{T}_{t=1} $$, where for each D-dimensional input $x^{(t)}$, each input dimension $$x^{(t)}_{d} \in \{0, 1\}$$. One motivation would be to learn the hidden representation of the inputs and in turn estimate the $$p(x^{(t)})$$ given any input $$x^{(t)}$$.

The authors state that typically for autoencoders and binary outputs, the binary cross entropy loss is employed during network training:

$$ l(x) = \sum_{d=1}^{D} -x_{d} \log \hat{x}_{d} -(1-x_{d})\log (1-\hat{x}_{d}) $$

The authors then highlight one consequence of using the loss function above, which is that if (in the worst case) the autoencoder just predicts a copy of the input then the predicted "probability" for any input is 1 and the sum of such "probabilities" considered over all input training images, would not sum to one (as it should do for a PDF), hence this loss function isn't infact a proper likelihood function. (This point and the publication is nicely explained by this [post](http://bjlkeng.github.io/posts/autoregressive-autoencoders/))

So what property could one impose on the autoencoder such that the output can be used to obtain valid probabilities? Here, the authors employ the chain rule and rewrite the loss function as follows:

$$ l(x) = \sum_{d=1}^{D} -x_{d} \log p(x_{d}=1 \vert x_{< d}) -(1-x_{d}) \log p \left( x_{d}=0 \vert x_{< d} \right) $$

Each output $$\hat{x}_{d} = p(x_{d} \vert x_{ < d})$$ must be a function taking as input only $x_{< d}$. This property is called an *autoregressive* property by the authors, because computing the negative log likelihood is equivalent to sequentially predicting each dimension of input $x$. Now since the output $$\hat{x}_{d}$$ depends only on the preceeding inputs $$x_{< d}$$, it means that there should be no computational path between the successive inputs $$x_{\geq d}$$ and $$\hat{x}_{d}$$. This is ensured by the authors through a clever 1-0 masking strategy.
