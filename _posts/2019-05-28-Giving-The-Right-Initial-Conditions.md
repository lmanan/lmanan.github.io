---
layout: post
title: Giving the Right Initial Conditions 
categories: Computer Vision
tags: 
mathjax: true
comments: true
---

We hypothesized that by an initial specification of a few correspondences, one can compute the affine transform needed to convert the fixed specimen to the live specimen image space. A few correspondences would do, since for an affine transform, merely three correspondences are needed in the 3-D space to solve for the underlying transform matrix. I believed such a precursor strategy would serve as a rough, initial step and aid the *Coherent Point Drift Algorithm* in giving more accurate results. 

In order to remove any uncertainty arising from spurious detections or outliers, I began with trying to establish the idea above on detections which are perfect and have been annotated by the user (i.e. one does not need to worry about biases that creep in due to the strategy employed for detection).

We used the number of nuclei as a proxy for finding *twins* between the two imaging modalities: since the number of nuclei is known perfectly due to manual annotation, we state that the fixed specimen at 16 hours post fertilization in question (Figure 1 (a)) should be as *old* as the time frame 333 from the time-lapse movie (Figure 1 (b)).

<p float="center"><figure>
<img src="/images/2019-05-28/01_Pdu_otxMHT_16hpf_pNA_PB_20180508-7_dapi.gif" width= "220" />
<img src="/images/2019-05-28/02_TM0333.gif" width ="220"/>
<figcaption>
[(From left to right) (a) Image shows a fixed specimen of *Platynereis* at 16 hours post fertilisation (Pdu_otxMHT_16hpf_pNA_PB_20180508-7_dapi.tif), stained for DAPI and imaged under a confocal microscope (b) Image shows a time frame number 333 from the time-lapse movie, which carries as many nuclei as shown in (a)]</figcaption>
</figure>
</p>

Since the live specimen is imaged less frequently in the z-dimension, we use the plugin *TransformJ* to resample it isotropically (this step is optional). Next, we use the plugin *3D Viewer* to apply individual transformation matrices which orient both embryos such that the ventral side faces the observer and the anterior-posterior axis of the embryo is along the vertical axis of the viewer's screen. The images of the two embryos look as in Figure 2. This sets the stage for identifying correspondences in the plugin *Mastodon*.  


<p float="center"><figure>
<img src="/images/2019-05-28/03_Pdu_otxMHT_16hpf_pNA_PB_20180508-7_dapi_transformed.gif" width= "220" />
<img src="/images/2019-05-28/04_TM0333_affined_transformed.gif" width ="220"/>
<figcaption>
[(From left to right) (a) Oriented fixed specimen (b) Oriented live specimen]</figcaption>
</figure>
</p>
 
