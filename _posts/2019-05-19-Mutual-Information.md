---
layout: post
title: Mutual Information 
categories: Computer Vision
tags: 
mathjax: true
comments: true
---



(From [Viola and Wells, 1995](http://people.csail.mit.edu/sw/papers/IJCV-97.pdf))
> "...  in several important applications the image data may not be a function of the model.  This is frequently the case in medical registration applications.  For example, a CT scan is neither a function of an MRI scan, nor is an MRI scan a function of a CT scan. Rather than require that the image be a function of the model, one natural generalization is to require that the image be predictable from the model.  Predictability is closely related to the concept of entropy. A predictable random variable has low entropy, while an unpredictable random variable has high entropy.  By moving to a formulation of alignment that is based on entropy, many of the drawbacks of weighted neighbor likelihood can be circumvented."

Mutual Information is frequently used as a metric for evaluating the quality of inter-modal registration (for example, see [Viola and Wells](http://people.csail.mit.edu/sw/papers/IJCV-97.pdf), [Tomer et al](https://www.ncbi.nlm.nih.gov/pubmed/20813265), [Thevenaz and Unser](https://infoscience.epfl.ch/record/63070/files/thevenaz0003.pdf)). In situations, where both correspondences and the underlying transformation are unknown, an iterative scheme can be adopted which estimates these two quantities in a coupled fashion, by maximizing mutual information at each step of the iteration. 

I was curious to get a better feeling of mutual information. In this experiment, I considered the correspondences as a *known* quantity and employed a Thin Plate Splines (TPS) model to warp a volumetric image into another. My aim was merely to see if this process lead to an increment of the mutual information metric between the transformed source image (scene) and the target image (model). Another objective was to shed more light on whether the non-linear part of a volumetric, TPS model should be characterized by $$r^{2} log(r)$$ or simply $$r$$ ([Jian and Vemuri](https://ieeexplore.ieee.org/document/5674050) and [David Eberly](https://www.geometrictools.com/Documentation/ThinPlateSplines.pdf) argue for the radial basis function equal to $$r$$ in a 3D TPS transform).

As an example, I chose two volumetric images showing the cell nuclei of a *Platynereis* embryo. These stacks are not identical and are the result of an unknown transformation. Using the [Get Correspondences](https://github.com/malaalam/TPS-MI/tree/master/src/main/java/MyGUI) class, I manually annotated 15 corresponding pixels (nuclei). These *matching-pairs* of pixels allowed me to estimate the TPS transformation parameters.   

<p float="center">
<img src="/images/2019-05-19/imageOne.gif" width= "350" /> 
<img src="/images/2019-05-19/imageTwo.gif" width ="350"/>
</p>

The next step involved generating the transformed form of *Image One*. The TPS transformation is not invertible, so Gradient Descent was used to go from the target image space to the source image space. As an example, I demonstrate [here](https://github.com/malaalam/TPS-MI/blob/master/src/test/resources/files/3D/01_matlab_scripts/estimateThinPlateSplineTransform.m) that for an arbitrary point in the target image space (highlighted as a red crosshair on the right side of the image), one can recover the corresponding pixel in the source image space (green crosshair, on the left side of the image).

<p><figure><img src="/images/2019-05-19/gradientDescent.gif" alt=""/><figcaption>
   [An *ImgLib2* Cursor is employed to crawl across the pixels in the transformed image space. For each such pixel, an inverse transform is calculated using the Gradient Descent technique. Once the corresponding pixel in the Source Image is found, its pixel intensity value is returned to populate the Transformed Image Space.]</figcaption></figure></p>

<p><figure><img src="/images/2019-05-19/TPS-MI.png" alt=""/><figcaption>
   [A screenshot of the TPS-MI Command:  it computes a transformed version of *Image One* and then compares it with *Image Two* for the evaluation of Mutual Information Metric]</figcaption></figure></p>

Upon running the [TPS-MI](https://github.com/malaalam/TPS-MI/blob/master/src/main/java/MyGUI/TPSCommand.java) command for 30 iterations at a learning rate equal to 0.1, the transformed version of *Image One* (left) looks quite similar to *Image Two* (right). 

<p float="center">
<img src="/images/2019-05-19/transformedImageOne.gif" width= "350" /> 
<img src="/images/2019-05-19/imageTwo.gif" width ="350"/>
</p>










 



