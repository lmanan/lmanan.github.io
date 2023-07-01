---
layout: post
title: Follow the Flow
categories: Computer Vision
tags:
mathjax: true
comments: true
---
<p><figure><img src="/images/2020-05-02/cellpose.png" alt=""/><figcaption>
[Source: Stringer et al, 2020. Example test image segmentation for *Cellpose*, *Stardist* and *Mask R-CNN*, when trained as specialist model]</figcaption></figure></p>

(From [Stringer et al](https://www.biorxiv.org/content/10.1101/2020.02.02.931238v2.full.pdf), 2020)
>"In the heat diffusion simulation, we introduce a heat source at the center pixel, which adds a constant value of 1 to that pixel's value at each iteration.   Every pixel inside the cell gets assigned the average value of pixels in a 3x3 square surrounding it,  including itself,  at every iteration, with pixels outside of a mask being assigned to 0 at every iteration.  In other words, the boundaries of the cell mask are *leaky*.  This process gets repeated for *N* iterations, where *N* is chosen for each mask as twice the sum of its horizontal and vertical range,  to ensure that the heat dissipates to the furthest corners of the cell.  The distribution of heat at the end of the simulation approaches the equilibrium distribution. We use this final distribution as an energy function, whose horizontal and vertical gradients represent the two flow fields that in our auxiliary vector flow representation."

Stringer and colleagues came up with an innovative strategy of producing an auxiliary representation of a ground truth cell mask by generating a vector flow field. They solved a steady state heat diffusion equation in the region of the mask of each cell, with a constant source kept at the *centre* of the cell and the background assumed to be a perfect sink.

The heat diffusion equation in such a scenario would be :

$$ q_{s} = - \alpha \left( \frac{\partial^{2} T}{\partial x^{2}}  + \frac{\partial^{2} T}{\partial y^{2}}  \right) $$

Here $q_{s}$ is the constant heat source, $T$ indicates the temperature at any position (x, y) and $$\alpha$$ includes factors like conductivity - which determine how fast the heat reaches from the source to the extremities. Since this is a second order differential equation in space, boundary conditions need to be specified. As I understand, the authors decided for a *Dirichlet* boundary condition, where the temperature of the boundary of the mask is set to 0 (i.e. a perfect sink?). I wonder if a *Neumann* boundary condition suggesting that $$\frac{\partial T}{\partial x_{\bot}} = 0 $$ would be better especially in the context of regions where cells are touching one other. (Not sure!)


Once the vector field is obtained, the authors *follow the flow* by looking at where each pixel is pointing to, following that direction and seeing which all pixels ended up at the same starting pixel. This is a novel idea! I wonder if there is any guarantee stemming from the heat diffusion equation that such vector fields obtained for the mask of one cell would only point to *one source*.

     


