---
layout: post
title: Analog Bits
categories: Computer Vision
tags:
mathjax: true
comments: true
---

[Chen et al, 2023](https://arxiv.org/pdf/2208.04202.pdf) present *Bit Diffusion* which is a simple and generic approach for generating discrete data using continuous time diffusion models. The idea is simple - convert discrete data to binary bits, scale these binary bits to be real numbers and then train a continuous diffusion model to model these bits (which the authors call as analog bits).

The figure 1 from their paper below represents the training and the sampling process well:

<p><figure><img src="/images/2021-04-25/analog-bits.png" alt=""/></figure></p>


This approach has great potential to capture discrete instance segmentation labels. However, the challenge there is that the value of the labels which are assigned to individual objects can be permuted without changing the segmentation result.
This is highlighted in the followup work called Pix2Seq-D by [Chen et al, 2023](https://arxiv.org/pdf/2210.06366.pdf). The authors say:

>"While the class categories of semantic labels are fixed a priori, the instance IDs assigned to objects in an image can be permuted without affecting the instances identified. For example, swapping instance IDs of two cars would not affect the outcome. Thus, a neural network trained to predict instance IDs should be able to learn a one-to-many mapping, from a single image to multiple instance ID assignments."

<p><figure><img src="/images/2021-04-25/pix2seq-d.png" alt=""/></figure></p>

Similar to Pix2Seq, in Pix2Seq-D, the authors model the likelihood of the mask conditioned on the current image. This can be extended to the task of tracking by also conditioning on the previous image which has the consequence of generating labels which are consistent across time (i.e. what is desired for object tracking).
