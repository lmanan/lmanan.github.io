---
layout: post
title: The Brachistochrone Problem 
categories: Computer Vision
tags: 
mathjax: true
comments: true
---

(From [Johann Bernoulli, 1696](https://en.wikipedia.org/wiki/Brachistochrone_curve#cite_note-5))
> "I, Johann Bernoulli, address the most brilliant mathematicians in the world. Nothing is more attractive to intelligent people than an honest, challenging problem, whose possible solution will bestow fame and remain as a lasting monument. Following the example set by Pascal, Fermat, etc., I hope to gain the gratitude of the whole scientific community by placing before the finest mathematicians of our time a problem which will test their methods and the strength of their intellect. If someone communicates to me the solution of the proposed problem, I shall publicly declare him worthy of praise."
 
The Brachistochrone Problem appears to be one of the earliest cited problems where *Calculus of Variations* was applied. It goes as follows: "*Find the curve of the fastest descent on which a bead slides frictionlessly under the influence of a uniform gravitational field to a given end point in the shortest time*". In order to delve further into Calculus of Variations which features frequently in the solution for techniques such as Optical Flow, Coherent Point Drift et cetera, I thought it might be a good idea to go right to the source and understand how it was used for this particular problem. 

Using the law of conservation of energy, one states that: $$ \frac{1}{2}v^{2} + gy = \text{constant} $$. This implies that the velocity at any instant is purely a function of the vertical height $$y$$ alone, that is $$v= f(y)$$. In addition, one could state that the total time taken to travel from the initial to the final position is T = $$\int dt$$ = $$\int \frac{\mathrm{d} s}{v} = \int \frac{\mathrm{d}x \sqrt{ 1 + y^{'2}}}{f(y)}=  \int G(y, y^{'})\mathrm{d}x $$, where $$y^{'} = \frac{\mathrm{d}y}{\mathrm{d}x}$$.

This problem becomes a prime candidate for the application of the Euler-Lagrange Equation since the nature of the relationship between the vertical and horizontal displacement of the weighted particle is unknown. In order to solve for this, we introduce the general case of the equation, as done [here](http://www.math.utah.edu/~cherk/teach/5710-03/print10-19.pdf): $$ \text{min}_{y} T(y) \text{ such that } T(y) = \int_{a}^{b} G( x, y, y^{'} ) dx, \text{ and } y(a) = y_{1}, y(b)=y_{2}$$. We suppose that $$y_{0} = y_{0}(x)$$ is a minimizer. If $$y_{0}$$ is indeed a minimizer, then $$ \delta T(y=y_{0}) = \int_{a}^{b} \left( \frac{\partial G}{\partial y} \delta y + \frac{\partial G}{\partial y^{'}} \delta y^{'}\right) \mathrm{d}x$$ = 0

Integrating the second term by parts gives us:
$$\delta T(y=y_{0}) = \int_{a}^{b} \left( \frac{\partial G}{\partial y} \delta y \right) \mathrm{d}x + \left( \frac{\partial G}{\partial y^{'}} \delta y|_{a}^{b} \right) - \int_{a}^{b} \left( \delta y|_{a}^{b} \frac{\mathrm{d}}{\mathrm{d}x} \frac{\partial G}{\partial y^{'}} \right) \mathrm{d}x$$ = 0

This (non-integral) term equals 0, since the boundary values of y are already prescribed, therefore their variations equal zero. Also, due to the arbitrariness of $$\delta y_{0}$$, we conclude that any differentiable minimizer $$y_{0}$$ solves the boundary-value problem and hence:
$$ \frac{\partial G}{\partial y} -  \frac{\mathrm{d}}{\mathrm{d}x} \frac{\partial G}{\partial y^{'}}   = 0$$

For a problem involving second order derivatives, for example $$ \text{min}_{y} T(y) \text{ such that } T(y) = \int_{a}^{b} G( x, y, y^{'}, y^{''} ) dx, \text{ and } y(a) = y_{1}, y(b)=y_{2}$$, the solution turns out to be:
$$ \frac{\partial G}{\partial y} -  \frac{\mathrm{d}}{\mathrm{d}x} \frac{\partial G}{\partial y^{'}} + \frac{\mathrm{d^{2}}}{\mathrm{d}x^{2}} \frac{\partial G}{\partial y^{''}}= 0$$, which suggests the general rule for the family of higher-order derivatives. 

For a function $$G = G( y, y^{'})$$ such as the one obtained above in the solution to the Brachistochrone Problem, one can show $$ y^{'} \frac{\partial G}{\partial y^{'}} - G = \text{constant}$$, which provides us the differential equation for the trajectory $$y= f(x)$$ of the bead:
$$\frac{1}{\sqrt{\text{constant}- gy}} = \sqrt{1 + y^{'2}}$$. This upon integration yields the equation of a **cycloid** as the *solution* to the problem of finding the curve of the fastest descent on which a bead slides frictionlessly. Pretty neat, huh?!



