---
layout: post
title: Learning Deformable Conditional Templates
categories: Computer Vision
tags:
mathjax: true
comments: true
---
(From [Dalca et al](https://arxiv.org/pdf/1908.02738.pdf), 2019)
>"A template can be chosen as one of the images in a given dataset, but often these do not represent the structural variability and complexity in the image collection, and can lead to biased and misleading analyses. If the template does not adequately represent dataset variability, such as the possible anatomy, it becomes challenging to accurately deform the template to some images. A good template therefore minimizes the geometric distance to all images in a dataset, and there has been extensive methodological development for finding such a central template. However, these templates are obtained through a costly global optimization procedure and domain-specific heuristics, requiring extensive runtimes. For complex 3D images such as MRI, this process can consume days to weeks. In practice, this leads to few templates being constructed, and researchers often use templates that are not optimal for their dataset. Our work makes it easy and computationally efficient to generate deformable templates."


Dalca and colleagues develop a probablistic approach of jointly synthesizing a new template and providing the defomation field to any image. The authors argue that no single image in a dataset should be chosen as the template as that would sacrifice the inherent variability in the population. Also, many of the present methods for building a template / atlas follow an iterative approach which is time-intensive and often leads to just one template being used to represent a wide population (instead of potentially more) in order to save time, which could lead to erroneous conclusions. This approach is especially useful if no template is currently available.

<p><figure><img src="/images/2020-04-20/poster.png" alt="" width="100%" height= "100%"/><figcaption>
   [Source: voxelmorph.mit.edu]</figcaption></figure></p>

The authors model each image $x_{i}$ as a deformation $\phi_{\nu_{i}}$ of a global template $t$.The template $t$ is next modeled as a function of some attribute $ a$ as follows  $t= f_{\theta_{t}}(a)$. For every datapoint, the deformable template parameters are estimated using maximum likelihood approach: 

$$\hat{\theta_{t}}, \hat{V} = \text{arg max} \text{log} p_{\theta_{t}} \left( V|X,A \right)$$


