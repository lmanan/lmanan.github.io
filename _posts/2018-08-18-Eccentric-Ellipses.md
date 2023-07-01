---
layout: post
title: Eccentric Ellipses
categories: Computer Vision
tags: 
comments: true
---

[Kong, Akakin and Sarma (2013)](https://www.researchgate.net/profile/Hatice_Cinar_Akakin/publication/260586629_A_Generalized_Laplacian_of_Gaussian_Filter_for_Blob_Detection_and_Its_Applications/links/561b60c108aea8036722beea/A-Generalized-Laplacian-of-Gaussian-Filter-for-Blob-Detection-and-Its-Applications.pdf) used the Laplacian differential detector, applied to the anisotropic form of the Gaussian, in order to detect elliptically-shaped biological cells which are oriented arbitrarily. The decision of using a second-order derivative follows from the intuition that the "Laplacian highlights regions of rapid intensity change, and as the scale (\\( \sigma \\))  of the LOG increases, blob like structures converge to a local extrema value". A schematic highlighting the extraction of the extrema of the scale-normalized response along the scale dimension is depicted in the figure below, taken from [Lindeberg (2015)](https://link.springer.com/content/pdf/10.1007%2Fs10851-014-0541-0.pdf):

<p><figure><img src="/images/2018-08-18/Lindeberg.PNG" width = "75%" height = "75%" alt=""/><figcaption>
   [Figure depicting the fifty strongest features linked by scale in blue and the "selected scales" corresponding to the global extrema of the scale-normalized response along each feature trajectory in red]</figcaption></figure></p>

Kong and colleagues opted for a set of **discrete** kernels which spanned a range of scales and orientations, and (probably) chose not to employ **steerability** for reducing the number of potential candidates in the search space. 

<p><figure><img src="/images/2018-08-18/Kong.PNG" width = "75%" height = "75%" alt=""/><figcaption>
   [Figure depicting the set of sixty-six kernels used by Kong and colleagues. The scales along the major and minor axes, in addition to the orientation of the ellipse are parameters which are varied between the various members of the set]</figcaption></figure></p>

Lindeberg and Garding had earlier in 1997, worked on a related problem (**Shape-Adapted Smoothing**). One could argue that their work led to the detection of elliptic blobs, although they pitched their idea as a solution to reducing shape distortion, caused previously by smoothing with a uniform Gaussian kernel. In the abstract, for example, they stated:

> " ... surface orientation will be biased due to the use of rotationally symmetric smoothing in the image domain. These effects can be reduced by extending the linear scale space concept into an affine Gaussian Scale space representation and by performing affine shape adaptation of the smoothing kernels." 

It appears to me that they used the default (linear) scale space theory to determine interesting points, at which a differential operator such as LOG would attain a maximum; and then proceeded to evaluate the **second moment matrix** in the local neighborhood of these points, to converge to the elliptic shape centred at those points. 

I wonder though why they did not directly employ the local covariance matrix, and instead opted for the second moment matrix (interesting properties of the second moment matrix, maybe?) Perhaps, knowing that the selection of interesting points based upon the linear scale space theory would introduce shape distortion, motivated Kong and colleagues to not use shape-adapted smoothing?



