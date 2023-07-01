---
layout: post
title: Quarternions
categories: Computer Vision
tags:
mathjax: true
comments: true
---
(From [Berthold K.P. Horn, 1987](https://www.osapublishing.org/josaa/abstract.cfm?uri=josaa-4-4-629))
> "There are many ways to represent rotation, including the following: Euler angles, Gibbs vector, Cayley-Klein parameters, Pauli spin matrices, axis and angle, orthonormal matrices, and Hamilton's quaternions. Of these representations, orthonormal matrices have been used most often in photogrammetry and robotics. There are a number of advantages, however, to the unit-quaternion notation. One of these is that it is much simpler to enforce the constraint that a quaternion has unit magnitude than it is to ensure that a matrix is  orthonormal. Also, unit quaternions are closely allied to the geometrically intuitive axis and angle notation"

Recently, I came to the realization that estimating a transformation between pairs of measurements coming from two coordinate systems should potentially not be performed using a Pseudo-Inverse operation - as it may not necessarily enforce an orthonormality constraint on the underlying rotation matrix. This lead me to Horn's work, where in he introduced a closed-form solution to estimating a rigid-body transformation between pairs of measurements, using quarternions. Horn argued that expressing a rotation operation as a quarternion was more convenient than expressing it as a rotation matrix, as then one needed to only ensure that the magnitude of the quaternion is equal to one (alternatively, one would ensure that the rotation matrix is orthonormal, which is a more involved process?!).

>"The transformation between two Cartesian coordinate systems can be thought of as the result of a rigid-body motion and can thus be decomposed into a rotation and a translation. In stereophotogrammetry, in addition, the scale may not be known. There are obviously three degrees of freedom to translation. Rotation has another three (direction of the axis about which the rotation takes place plus the angle of rotation about this axis). Scaling adds one more degree of freedom. Three points known in both coordinate systems provide nine constraints (three coordinates each), more than enough to permit determination of the seven unknowns."

Horn mentioned that three pairs of points were sufficient to estimate the correct rigid body transformation. He formulated the problem in terms of the measured coordinates in the left and the right hand coordinate system, which are denoted as $$\mathbf{r_{l, i}}$$ and $$\mathbf{r_{r,i}}$$. 


Following is a summary of some of the steps from his work. 

We are looking for a transformation of the form:

$$\mathbf{r_{r}} = sR(\mathbf{r_{l}}) + \mathbf{r_{0}}$$

Unless the data is perfect, there will be a residual error:

$$ \mathbf{e_{i}} = \mathbf{r_{r, i}} - s R(\mathbf{r_{l,i}}) - \mathbf{r_{0}} $$

We will minimize the sum of these errors:

$$ \sum_{i=1}^{n} ||\mathbf{e_{i}} ||^{2} $$

It turns out useful to refer all measurements to the centroids:

$$\vec{\mathbf{r_{l}}}= \frac{1}{n} \sum_{i=1}^{n} \mathbf{r_{l,i}}$$

$$\vec{\mathbf{r_{r}}}= \frac{1}{n} \sum_{i=1}^{n} \mathbf{r_{r,i}}$$

## Estimation of Translation

Let us denote the new coordinates by:

$$\mathbf{r_{l,i}^{'}} = \mathbf{r_{l,i}} - \vec{\mathbf{r_{l}}}$$

$$\mathbf{r_{r,i}^{'}} = \mathbf{r_{r,i}} - \vec{\mathbf{r_{r}}}$$

Hence, the error term can be re-written as:

$$\mathbf{e_{i}}=\mathbf{r^{'}_{r, i}} - s R(\mathbf{r^{'}_{l,i}}) - \mathbf{r^{'}_{0}} $$ 

where 

$$\mathbf{r^{'}_{0}} = \mathbf{r}_{0} - \vec{\mathbf{r_{r}}} + sR(\vec{\mathbf{r_{l}}})$$

The sum of square of errors becomes:

$$ \sum_{i=1}^{n} \vert\vert \mathbf{r_{r,i}^{'}} - s R(\mathbf{r_{l,i}^{'}}) - \mathbf{r_{0}^{'}} \vert \vert^{2} $$

or

$$ \sum_{i=1}^{n} \vert \vert \mathbf{r_{r,i}^{'}} - sR(\mathbf{r_{l,i}^{'}}) \vert \vert^{2} - 2 \mathbf{r_{0}^{'}} \sum_{i=1}^{n} \left[ \mathbf{r_{r, i}^{'}} - sR(\mathbf{r^{'}_{l,i}} ) \right] + n \vert\vert \mathbf{r_{0}^{'}} \vert \vert^{2} $$

In order to minimize this  wrt $$\mathbf{r_{0}}$$, 

$$ \vert\vert \mathbf{r_{0}^{'}} \vert \vert $$ = 0

this implies 

$$ \vec{\mathbf{r_{r}}} + sR(\vec{\mathbf{r_{l}}}) = \mathbf{r}_{0} $$

## Estimation of Scale 

$$ s = \left(   \frac{ \sum_{i=1}^{n} \vert \vert \mathbf{r_{r,i}^{'}\vert\vert^{2}}}{ \sum_{i=1}^{n} \vert \vert \mathbf{r_{l,i}^{'}\vert\vert^{2}} } \right) $$


## Estimation of Rotation


