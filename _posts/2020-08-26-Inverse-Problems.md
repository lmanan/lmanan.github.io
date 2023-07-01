---
layout: post
title: Inverse Problems for Supervised Learning
categories: Computer Vision
tags:
mathjax: true
comments: true
---



(From [Adler and colleagues, 2017](https://arxiv.org/abs/1704.04058))
>"Using supervised machine-learning to solve inverse problems in imaging requires training data where ground truth images are paired with corresponding noisy indirect observations. The learning provides a mapping that associates observations to corresponding images. However, in several applications there are difficulties in obtaining the ground truth, e.g., in many cases it may have undergone a distortion. For example, a recent study showed that MRI images may be distorted by up to 4 mm due to, e.g., inhomogeneities in the main magnetic field. If these images are used for training, the learned MRI reconstruction will suffer in quality. Similar geometric inaccuracies arise in several other imaging modalities, such as Cone Beam CT and full waveform inversion in seismic imaging."

Adler and colleagues suggest that often in  supervised denoising tasks, one needs to estimate model weights from noisy and clean observations. This requires a finely registered noisy-clean pair of images, but this may in reality, be hard to obtain. If such distorted ground truth images are used, then the learned restoration may suffer in quality.

