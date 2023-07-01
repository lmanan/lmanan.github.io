---
layout: post
title: NoiseFlow
categories: Computer Vision
tags:
mathjax: true
comments: true
---



(From [Abdelhamed and colleagues, 2019](https://openaccess.thecvf.com/content_ICCV_2019/papers/Abdelhamed_Noise_Flow_Noise_Modeling_With_Conditional_Normalizing_Flows_ICCV_2019_paper.pdf))
>"We introduce Noise Flow, a new noise model that combines the insights of parametric noise models and the expressiveness of powerful generative models.Specifically, we leverage recent normalizing flow architectures to accurately model noise distributions observed from large datasets of real noisy images. In particular, basedon the recent Glow architecture, we construct a normalizing flow model which is conditioned on critical variables, such as intensity, camera type, and gain settings (i.e. ISO). The model can be shown to be a strict generalization of the camera NLF but with the ability to capture significantly more complex behaviour. The result is a single model that is compact (fewer then 2500 parameters) and considerably more accurate than existing models."



[Abdelhamed and colleagues](https://openaccess.thecvf.com/content_ICCV_2019/papers/Abdelhamed_Noise_Flow_Noise_Modeling_With_Conditional_Normalizing_Flows_ICCV_2019_paper.pdf) provide a cool, unsupervised technique to perfom denoising of natural images. They introduce a method called as `NoiseFlow`, which allows them to represent the noise model with ~ 2500 parameters. 

The authors begin by introducing the different types of noise models previously available. For example, a uni-variate `homoscedastic` Gaussian model represents the noisy intensity $x_{i}$ as $x_{i} = s_{i} + N(0, \sigma^{2})$ Such a model does not include the notion that *photon noise* is signal dependent - here the noise is modeled independent of the underlying signal intensity.

On the other hand, the authors argue that more powerful, signal-dependent `heteroscedastic` noise models fail to capture *fixed-pattern noise* and *non-linearities*, such as amplification noise and quantization. Hence, `NoiseFlow` is proposed which brings together the physical insights of a parametric noise model but also the flexibility of a generative model.

>"Signal-dependent models may accurately describe noise components, such as photon noise. However, in real images there are still other noise sources that may not be accurately represented by such models. Examplesof such sources include fixed-pattern noise, defective pixels, clipped intensities, spatially correlated noise (i.e., cross-talk), amplification, and quantization noise."

I realize that I am not very familiar with the origin of some of these noise sources, so should read up! This figure taken from the publication was, I thought a wonderful summary of the different source s of noise!


<p><figure><img src="/images/2020-10-07/sourcesOfNoise.png" alt=""/><figcaption>
[Source: Abdelhamed et al, 2019. Sources of Noise ]</figcaption></figure></p>
