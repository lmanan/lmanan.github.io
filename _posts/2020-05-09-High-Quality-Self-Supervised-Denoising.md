---
layout: post
title: High Quality Self Supervised Image Denoising
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-05-09/laine.png" alt=""/><figcaption>
[Source: Laine et al, 2019. ]</figcaption></figure></p>

(From [Laine et al](https://arxiv.org/pdf/1901.10277.pdf), 2019)
>"The networks used by Krull et al. do not have a blind spot by design, but are trained to ignore the center pixel using a masking scheme where only a few output pixels can contribute to the loss function, reducing training efficiency considerably.  We remedy this with a novel architecture that allows efficient training without masking. Furthermore, the existence of the blind spot leads to poor denoising quality.   We derive a scheme for combining the network output with data in the blindspot, bringing the denoising quality on par with, or at least much closer to, conventionally trained networks."
