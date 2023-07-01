---
layout: post
title: Kalman Filter for Tracking 
categories: Computer Vision
tags:
mathjax: true
comments: true
---
Much of the text below is inspired from [here](https://refubium.fu-berlin.de/bitstream/handle/fub188/19186/2005_12.pdf?sequence=1).

Kalman Filter provides a recursive solution to the linear, optimal filtering problem. It applies to both stationary and non-stationary environments.The solution is recursive so that each updated solution is calculated from the previous estimate and the new input data, so that only the previous estimate needs storage. 

Consider a linear, discrete time dyamical system. The following system of equations are produced:

1) Process Equation:
$$x_{k+1} = F_{k+1, k} x_{k} + w_{k}$$ 

Here, $$F_{k+1, k}$$ is the transition matrix taking the state from $$k$$ to $$k+1$$. The process noise $$w_{k}$$ is assumed to be additive, white and gaussian with zero mean.

2) Measurement Equation
$$y_{k} = H_{k} x_{k} + v_{k}$$ 

The $$y_{k}$$ is assumed to be the observable at time k and $$H_{k}$$ is the measurement matrix. The measurement noise $$v_{k}$$ is assumed to be white, gaussian with zero mean.

How is the Kalman filter typically extended for tracking of multiple objects? How is non-Gaussian noise in the measurement or observation usually handled?
 
