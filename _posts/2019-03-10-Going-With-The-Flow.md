---
layout: post
title: Going with the Flow 
categories: Computer Vision
tags: 
mathjax: true
comments: true
---

(From [Horn and Schunk, 1981](http://image.diku.dk/imagecanon/material/HornSchunckOptical_Flow.pdf))
> "The relationship between the optical flow in the image plane and the velocities of objects in the three dimensional world is not necessarily obvious. We perceive motion when a changing picture is projected onto a stationary screen, for example. Conversely, a moving object may give rise to a constant brightness pattern. Consider, for example, a uniform sphere which exhibits shading because  its  surface elements are oriented in many different directions. Yet, when it is rotated, the optical flow is zero at all points in the image, since the shading does not  move with the surface. Also, specular reflections move with a velocity characteristic of the virtual  image,  not the surface in which  light is reflected."
 
It appears to me that the theory of optical flow was initially designed for the purpose of studying the motion of real world objects by projecting them on a two-dimensional space. I wonder how the theory has evolved over the years, in order to cater to the large number of 3-D microscopy datasets in the last decade. 

A surprising feature of the H-S model for me is their inclusion of the Smoothness Constraint. The authors state that the sum of the laplacians of the velocity components should be minimized. I wonder if it would not have been a better idea to invoke the continuity equation from incompressible, fluid dynamics and claim that the absolute magnitude of $$  \frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}  $$  should be minimized.

[Brox and Malik](https://lmb.informatik.uni-freiburg.de/people/brox/pub/brox_tpami10_ldof.pdf) comment on Horn and Schunk's seminal work - " ... **local**, gradient based matching of pixel gray values is combined with a **global** smoothness assumption". I wonder why do they interpret the regularization term as suggestive of including global information, since the Laplacians of the velocity components are evaluated using the (local) neighbouring pixels. Brox and Malik's interpretation of the local constancy of intensity equation as equivalent of reducing differences between corresponding pixels appears logical. It also follows hence that by replacing the linear, Taylor's assumption with a higher-order assumption, one can estimate optical flow for larger displacements between consecutive time frames. 



