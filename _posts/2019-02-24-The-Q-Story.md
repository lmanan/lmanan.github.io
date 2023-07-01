---
layout: post
title: The Q Story 
categories: Computer Vision
tags: 
mathjax: true
comments: true
---

Probabilistic graph matching tries to find the optimum parameters that explains the transformation between two clouds of points. This question is analogous to fitting one cloud of points (X) to another cloud modelled as a mixture of gaussians (Y). The optimum parameters in this case would include the transformation parameters for rotation, scaling, translation and shearing, and may also include the variances of the gaussians, as was done in the [Coherent Point Drift](https://arxiv.org/pdf/0905.2635.pdf).  

I considered using the same technique for finding the analogous correspondences between features extracted from the nuclei imaged live or fixed. For feature extraction, I employed the Laplacian of Gaussian, Scale Space Approach, which in addition to localising the feature, also returns information about its size (scale). In such a scenario, I reasoned that the variances of the gaussians are no longer parameters which need to be estimated, as the optimum scale values for the gaussians are already known.

As presented in this [tutorial](http://www.cs.tau.ac.il/%7Ershamir/algmb/archive/EM-BW.pdf), our goal is to find parameters $$\theta $$ that maximise the likelihood:

$$
L (\theta) = \prod_{i=1}^{N} P(x_{i} |\theta) = \prod_{i=1}^{N} \sum_{j} P(x_{i}, y_{i} = j | \theta) = \prod_{i=1}^{N} \sum_{j} P(y_{i} =j | \theta) P(x_{i}| y_{i}=j, \theta)
$$


> "The first equality results from the independence of the samples in X; the second equality results from the law of total probability, and the last equality is due to Bayes’ theorem. Finding the maximum of the resulting log likelihood function is therefore not trivial as its partial derivatives depend both on the model parameters and on the latent variables."

Instead of finding $$\theta $$ that maximises the likelihood, we use the EM approach to find $$\theta $$ that obtains a local maximum of the likelihood. We will denote the model parameters of iteration $$ t$$ as $$ \theta^{t}$$, and show that given these parameters we can choose the parameters of the next iteration $$ \theta$$ such that:

$$ \mathrm{log} P(X | \theta) \geq \mathrm{log} P(X | \theta^{t}) $$

By Bayes theorem, we have:
$$ P (y_{i}= j | x_{i}) = \frac{P(y_{i}=j) P(x_{i}| y_{i}=j)}{P(x_{i})} $$

Hence,
$$ P (y_{i}= j | x_{i}, \theta) = \frac{P(y_{i}=j |\theta ) P(x_{i}| y_{i}=j, \theta )}{P(x_{i} | \theta)} $$

Thus,
$$ P(x_{i} | \theta)  = \frac{P(y_{i}=j |\theta ) P(x_{i}| y_{i}=j, \theta )}{
P (y_{i}= j | x_{i}, \theta)} = \frac{P( x_{i}, y_{i} = j | \theta)}{P (y_{i}= j | x_{i}, \theta)}$$
 
Taking a log of both sides:

$$ \mathrm{log} P(x_{i} | \theta) = \mathrm{log} P(x_{i}, y_{i} = j | \theta) - \mathrm{log} P (y_{i} = j | x_{i}, \theta) $$

Multiplying both sides:

$$ P (y_{i}= j | x_{i}, \theta^{t}) \mathrm{log} P(x_{i} | \theta) = P (y_{i}= j | x_{i}, \theta^{t}) \mathrm{log} P(x_{i}, y_{i} = j | \theta) - P (y_{i}= j | x_{i}, \theta^{t}) \mathrm{log} P (y_{i} = j | x_{i}, \theta) $$

Sum the above over for all y:

$$ \sum_{j} P (y_{i}= j | x_{i}, \theta^{t}) \mathrm{log} P(x_{i} | \theta) = \sum_{j} P (y_{i}= j | x_{i}, \theta^{t}) \mathrm{log} P(x_{i}, y_{i} = j | \theta) - \sum_{j} P (y_{i}= j | x_{i}, \theta^{t}) \mathrm{log} P (y_{i} = j | x_{i}, \theta) $$

$$  \mathrm{log} P(x_{i} | \theta) =  \sum_{j} P (y_{i}= j | x_{i}, \theta^{t}) \mathrm{log} P(x_{i}, y_{i} = j | \theta) -  \sum_{j} P (y_{i}= j | x_{i}, \theta^{t}) \mathrm{log} P (y_{i} = j | x_{i}, \theta) $$

$$ Q( \theta | \theta^{t}) =  \sum_{j} P (y_{i}= j | x_{i}, \theta^{t}) \mathrm{log} P(x_{i}, y_{i} = j | \theta) $$

It appears that maximising the Q function is sufficient to ensure that:

$$ \mathrm{log} P(X | \theta) \geq \mathrm{log} P(X | \theta^{t}) $$

The Q function is akin to the expectation of the complete likelihood function. In the Coherent Point Drift paper, the complete Q function (in the absence of noise and outliers) takes the form:

$$ Q = -\sum_{i}^{N} \sum_{j}^{M} P(y_{i} = j | x_{i}, \theta^{t}) \mathrm{log} \left( P(y_{i} = j | \theta) P(x_{i} | y_{i} = j, \theta) \right) $$    
  
$$ Q = -\sum_{i}^{N} \sum_{j}^{M} P(y_{i} = j | x_{i}, \theta^{t}) \mathrm{log} \left( P(y_{i} = j | \theta) \right) -\sum_{i}^{N} \sum_{j}^{M} P(y_{i} = j | x_{i}, \theta^{t}) \mathrm{log} \left( P(x_{i} | y_{i} = j, \theta) \right) $$ 
 
$$ Q = -\sum_{i}^{N} \sum_{j}^{M} P(y_{i} = j | x_{i}, \theta^{t}) \mathrm{log} \left( P(y_{i} = j | \theta) \right) -\sum_{i}^{N} \sum_{j}^{M} P(y_{i} = j | x_{i}, \theta^{t}) \mathrm{log} \left( \frac{1}{\left(2 \pi \sigma_{j}^{2} \right)^{D/2}}\mathrm{exp}^{\frac{-\left( x_{i}-T(y_{i}=j) \right)^{2}}{2 \sigma_{j}^{2}}}  \right) $$

In the equation above, my formulation diverges from that of CPD, since I have a separate scale for each of the gaussians, and these are known parameters!

$$ Q = -\sum_{i}^{N} \sum_{j}^{M} P(y_{i} = j | x_{i}, \theta^{t}) \mathrm{log} \left( P(y_{i} = j | \theta) \right) + \frac{D}{2} \sum_{i}^{N} \sum_{j}^{M} P(y_{i} = j | x_{i}, \theta^{t}) \mathrm{log} \left(2 \pi \sigma_{j}^{2} \right)  + \sum_{i}^{N} \sum_{j}^{M} P(y_{i} = j | x_{i}, \theta^{t}) \frac{-\left( x_{i}-T(y_{i}=j) \right)^{2}}{2 \sigma_{j}^{2}} $$

[Myronenko and Song](https://arxiv.org/pdf/0905.2635.pdf) ignore the first term on the right hand side in Equation Number 5, since it is independent of the parameters. Also, the second term on the right hand side of the equation lacks the constant factor 2*pi in their formulation, for the same reason. In my case, we could totally ignore the second term for the purpose of optimisation since the scales are known values and not parameters to be estimated.

My next task would be to validate the partial derivatives of Q with respect to the parameters, in an affine transformation scenario (there is no reason to ignore shearing effects between specimens, imaged fixed and live).

<p><figure><img src="/images/2019-02-24/Q.jpg" alt="" width="50%" height= "50%"/><figcaption>
   [Desmond Llewelyn - The other famous Q]</figcaption></figure></p>

