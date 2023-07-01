---
layout: post
title: Inferring number of independent blinking emitters
categories: Biology
tags:
mathjax: true
comments: true
---

Inferring number of independent blinking emitters when imaged with light microscopy is a long-standing problem. This is an issue because the size of the molecular complexes is smaller than the diffraction limit and hence can not be resolved purely by using light microscopy. This motivates a need to have computational approaches for inferring the underlying number of complexes.

Although SMLM (single molecule localization microscopy) has a similar goal and relies on the assumption that only a few fluorophores would be active at any given time, the technique is suitable only for large complexes and would fail for nanometer-sized complexes.

Assume that the observed variable is the intensity at a given time point $x_{t}$ and the hidden variable which needs to be inferred is the number of active emitters at that time point $y_{t}$. 
 
Say if we were to evaluate $p(\textbf{x} ; n, \theta)$, where $n$ is the total number of emitters and  $\theta$ is the parameters of the model (more about this later), one could enforce a Markovian assumption:

$$
p(\textbf{x}; n, \theta) = \int p(\textbf{x}, \textbf{y}; n, \theta) d\textbf{y}
$$

$$
\propto \sum p(\textbf{x} | \textbf{y}; n, \theta) p(\textbf{y}; n, \theta)
$$

Using the Markov Assumption ...

$$
\propto \sum p(\textbf{x} | \textbf{y}; n, \theta) p(y_{1}; n, \theta) \prod^{t=T}_{t=2}  p(y_{t} | y_{t-1}; n, \theta)
$$

Then using the conditional independence assumption, one could say that $p(\textbf{x} \| \textbf{y}) = \prod_{t} p(x_{t} \| y_{t})$, and therefore:

$$
\propto \sum p(x_{1} | y_{1}; n, \theta) p(y_{1}; n, \theta) \prod^{t=T}_{t=2}  p(x_{t} | y_{t}; n, \theta) p(y_{1}; n, \theta)  p(y_{t} | y_{t-1}; n, \theta)
$$
