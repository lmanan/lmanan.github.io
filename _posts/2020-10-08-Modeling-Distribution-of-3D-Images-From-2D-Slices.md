---
layout: post
title: Modeling the Distribution of 3D Images
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Volokitin et al, 2020](https://arxiv.org/pdf/2007.04780.pdf))
>"By separately encoding  all  of  the  slices  coming  from  the  same  volume  using  our 2D  encoder,  over  many  different  volumes,  we  can  estimate  the  sample  mean and  covariance  of  the  latent  codes  over  the  slice  dimension.  This  gives  us  a model for 3D data and lets us sample from the distribution by generating a new stack of latent codes with the same mean and covariance as the original codes, which, when decoded, correspond to a new consistent MR volume."

Volokitin et al provide a novel approach to learn the distribution of volumetric images by leveraging the statistical dependence of the signal arising from different 2D slices. The authors train a 2D variational auto encoder network to convergence where each slice arising from one of the several 3D volumes is encoded to a L dimension point in the latent space. 


The authors formulate the following equations. Here, $t$ indicates the depth of the slice in the volumetric image:

In order to learn the distribution of a volume $V$, one requires $P_{\theta}(V)$. Here $\theta$ indicates the parameters of the decoder network. This can be obtained through factorisation as:

$$
P_{\theta}(V) = \Sigma_{\textbf{y}} P_{\theta}(V | \textbf{y}) P_{\theta}(\textbf{y})
$$

(where $\textbf{y} = [y_{0}  \ldots y_{l} \ldots y_{L}]$ . $l$ indicates the  $l^{\text{th}}$ dimension of the latent vector $\textbf{y}$ and $\textbf{y}$ is evaluated as : 
$$
\textbf{y}(t) =\text{encoder}( X(t))
$$
)

Now the Volume itself can be seen as a combination of several slices i.e.

$$ 
V = [X_{1} \ldots X_{t} \ldots X_{T}] 
$$

Hence, 

$$
P_{\theta}(V | \textbf{y}) = P_{\theta}(X_{1}, X_{2}, X_{3}, \ldots, X_{T}  | \textbf{y})
$$
 

The authors state that since each slice is decoded independently by the decoder $\theta$, hence it is okay to say that:

$$
P_{\theta}(V | \textbf{y}) = \prod^{t=T}_{t=1} P_{\theta}(X_{t}  | \textbf{y})
$$


Now, we come to evaluating $P_{\theta}(\textbf{y})$. Since $\textbf{y}= [y_{0} \ldots y_{l} \ldots y_{L}]$ and assuming independence of each latent dimension, implies

$$
P_{\theta}(\textbf{y}) = P_{\theta}(y_{0} \ldots y_{l} \ldots y_{L}) = \prod^{L}_{l=0} P_{\theta}(y_{l})
$$

By assuming that corresponding latent variables across different slices are statistically related, one could use a Gaussian Model to express this relation:



$$
p(y_{l}) = \mathcal{N}(y_{l} | \mu_{l},\Sigma_{l}) 
$$

Following this comes the interesting part. For simplicity, consider that the latent vector has only dimension i.e. L = 1. Now, if we collect the latent vectors from all slices at depth d = 0 and fit a gaussian distribution to estimate a $\mu (d = 0)$ and a $\Sigma (d =0)$,  and then we repeat the process to obtain the estimates for $\mu (d =5)$ and $\Sigma(d = 5)$, then we can expect that the fitted $\mu(d=0)$ and $\mu ( d=5)$ can not be too different. The similar belief holds for $\Sigma (d=0)$ and $\Sigma (d =5)$. Hence, this implies that the mean and the standard deviation can be considered as functions of the depth. One relation could be:

$$
\mu(d) = a + b \times d
$$

$$
\Sigma (d) = c + f \times d 
$$

This relation above implies that using the latent vector codes, one needs to estimate 4 parameters ($a$, $b$, $c$ and $f$) times L dimensions, and this enforces a parametric model explaining the gradual shift of the values of the latent vector with the depth of the slice. 

I am not actually sure if this is what the authors intended to do. Have to wait and watch for their code!

In any case, with a framework as above, generating a volume becomes easy. First,sample a $$z \sim \mathcal{N}(0, I)$$, then use $$y_{l} (d) = \mu_{l} (d) + \Sigma_{l}(d)^{1/2} z$$. This way, since $\mu_{l}$ is available for all depths (because of some parametric relation available, such as the one above), one can obtain a latent encoding for the $l^{th}$ dimension across all depths. Repeat this process for all the L dimensions and you should be able to generate a volume. 
