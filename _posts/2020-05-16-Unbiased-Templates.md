---
layout: post
title: Unbiased Templates
categories: Computer Vision
tags:
mathjax: true
comments: true
---

From [Joshi et al, 2004](https://www.sciencedirect.com/science/article/pii/S1053811904003842?casa_token=ImKrgmYUYDwAAAAA:bCWexezBcMVFsN3K2o8z0Ff5KbdstsPl73Hx1q0WSL_PLUVOEHKP70-_qM5p5s0Cc7F9h6ez)
>"These small deformation approaches are based on the assumption that a transformations of the form $h(x) = x + u(x)$, parameterized via a displacement field, u(x), are close enough to the identity transformation such that composition of two transformations can be approximated via the addition of their displacement fields:
$$h_{1} \circ h_{2} (x) \sim x + u_{1} (x) + u_{2} (x) $$"


Joshi and colleagues develop a methodology which allows simultaneous estimation of transformations and an unbiased template in the large deformation setting. This allows building atlases for populations with large geometric variability. The method developed by the authors does not assume the approximation as quoted above.

 
