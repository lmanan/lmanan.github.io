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


This approach has great potential to capture discrete instance segmentation labels.  

