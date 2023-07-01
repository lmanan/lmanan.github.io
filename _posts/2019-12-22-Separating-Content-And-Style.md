---
layout: post
title: Separating Content and Style 
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Gatys and colleagues, 2015](https://arxiv.org/pdf/1505.07376.pdf))
>"In general, there are two main approaches to find a texture generating process. The first approach is to generate a new texture by resampling either pixels or whole patches of the original texture.  These non-parametric resampling techniques and their numerous extensions and improvements are capable of producing high quality natural textures very efficiently. However, they do not define an actual model for natural textures but rather give a mechanistic procedure for how one can randomise a source texture without changing its perceptual properties. 
In  contrast, the second approach to texture synthesis is to explicitly define a parametric  texture model. The model usually consists of a set of statistical measurements that are taken over the spatial extent of the image. In the model, a texture is uniquely defined by the outcome of those measurements and every image that produces the same outcome should be perceived as the same texture.
"

Gatys and colleagues describe a new parametric approach to generating textures. They employ a CNN in order to obtain a  texture model that is parameterized by **spatially invariant** representations built on the **heirarchical processing** architecture of the CNN. Furthermore, in the [publication](https://arxiv.org/abs/1508.06576), the authors argue that representations of content and style can be separated by employing CNNs. That is, one can manipulate both these representations independently to produce meaningful images. 

To demonstrate this, they employ the VGG-19 network, pretrained on the [ImageNet](https://arxiv.org/abs/1409.0575) classification and detection benchmark. A white noise image is fed to the network and its intensities are iterated until the stage when the image converges to a state where its style matches an image, it was trying to mimic.  

I wondered whether such an approach could be used to simulate the texture of biological cells. Since a VGG19 network requires RGB (3 channel) images, and since images arriving from the microscope showing biological cells are typically one-channel, uint-16 type, maybe it makes sense to use a different architecture to implement this idea? 



