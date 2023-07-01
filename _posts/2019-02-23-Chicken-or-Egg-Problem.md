---
layout: post
title: Chicken or Egg Problem
categories: Computer Vision
tags: 
comments: true
---
(From [Uijlings et al, 2012](http://www.huppelen.nl/publications/selectiveSearchDraft.pdf)) 
> "For a long time, objects were sought to be delineated before their identification.  This gave rise to segmentation, which aims for a unique partitioning of the image through a generic algorithm, where there is one part for all object silhouettes in the image. But images are intrinsically heirarchical ... This has led to the opposite of the traditional approach: to do localisation through the identification of an object."
 
Uijlings et al makes an interesting observation about the chain of events that ensue before we recognize an object - do we segment all objects in the visual space, prior to assigning a recognition to each one of them or do we detect an object first and then carve out its contour, which completes the recognition process. (Uijlings thinks it is the latter).

The authors further talk about having an appearance model learned from examples and an exhaustive search which is performed where every location within the image is examined as to not miss any potential object location. I wonder how they implement such an appearance model and if it is similar to using a random forest of features?

The authors claim that an exhaustive search is not the way to go for since the visual space is massive (more so if you go from a two dimensional to three dimensional space) and vouch for a Selective Search regime. How do they cut down on probing all search locations?
