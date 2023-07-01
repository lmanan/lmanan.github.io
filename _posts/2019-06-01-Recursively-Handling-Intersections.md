---
layout: post
title: Recursively Handling Intersections
categories: Computer Vision
tags:
mathjax: true
comments: true
---

For a while, I battled with the choice of the most suitable property to prioritize when faced with several intersecting detections - for example, should I prefer larger structures over smaller; brighter over darker; higher, normalized Laplacian of Gaussian (LOG) - convolved intensity over lower. In the end, I went ahead with the latter-most, arguing to myself that a structure with a higher, normalized LOG - convolved intensity also represents a higher level of confidence in the detection.

<p float="center"><figure>
<img src="/images/2019-06-01/01_rawImage.gif" width= "220" /> 
<img src="/images/2019-06-01/02_beforeCC.gif" width ="220"/>
<img src="/images/2019-06-01/03_afterCC.gif" width ="220"/>
<figcaption>
[(From left to right) (a) Image shows a fixed specimen of *Platynereis* at 16 hours post fertilisation, stained for DAPI and imaged under a confocal microscope (b) Detected nuclei with a few intersections are seen in the center image. Here, LOG, Scale Space aproach was employed (c) Detected nuclei with no intersections are seen in the right-most image. Here, clusters of intersecting detections are processed recursively]</figcaption></figure>  
</p>

Spherical structures are detected using the classes [MainCommand](https://github.com/malaalam/DetectSphericalStructures/blob/master/src/main/java/bdv/ui/panel/MainCommand.java) and [BlobDetection](https://github.com/malaalam/DetectSphericalStructures/blob/master/src/main/java/bdv/ui/panel/instanceSegmentation/BlobDetection.java). These require as user inputs the range of scales (*object sizes*) that one would like to look for within an image; whether an image is undersampled in one of the axes and the corresponding (normalized) voxel dimension, and lastly, the information about if the spherical structures are brighter than the background or vice versa (this motivates a decision internally about looking for local minima and local maxima, respectively).

<p float="center"><figure>
<img src="/images/2019-06-01/04_detectSphericalStructures.png" width= "350" />
<img src="/images/2019-06-01/05_editDetections.png" width ="350"/>
<figcaption>
[(From left to right) (a) In the first step, nuclei are detected by specifying a range of scales or object sizes which are of interest in the image (b) Next, these detections are curated - either by manually adjusting the threshold and/or by suppressing intersections]</figcaption></figure>
</p>

The intersecting detections are handled in the **Edit Detections** Panel. The corresponding method of interest is provided [here](https://github.com/malaalam/DetectSphericalStructures/blob/612d756639e2864f0708e48b45f8ca685f8e1c11/src/main/java/bdv/ui/panel/instanceSegmentation/MyOverlay.java#L105). Essentially, a graph is constructed where each node is a detection, and which has edges between those pair of nodes (detections) that intersect with each other. Next, connected components are found using a Depth-First Search implementation, where each connected component corresponds to one or more nodes (detections). If the size of the connected component is one, then the singular node is passed on to an open list of *good* detections; otherwise, the members of the connected component are sorted in the descending order based on their individual, normalized LOG - convolved intensity, the first node is passed on to the list of *good* detections while the nodes with which it intersects, are discarded. This connected component is processed recursively until its number of members trickles to one.      
