---
layout: post
title: Joint Optimising Spatial Embeddings and Clustering Bandwidth
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-05-14/joint_optimisation.png" alt=""/><figcaption>
[Source: Neven et al, 2019. Instance Segmentation Pipeline. Bottom branch predicts (a) sigma value for each pixel, which translates into a clustering margin for each object. Bigger objects are bluish indicating a larger margin while smaller objects are yellowish, indicating smaller margins. (b) Offset vectors for each pixel pointing to the centre of attraction and displayed using a color encoding, where the color indicates the angle of the vector. The top branch produces a seedmap for each semantic class. A high value indicates that the offset vector points directly at the object center. Note that the border are bluish indicating that they have difficulty knowing which center to point to. The pixel embeddings and margins calculated from the predicted sigma are also shown. The cluster centers are derived from the seed maps]</figcaption></figure></p>

(From [Neven et al](http://openaccess.thecvf.com/content_CVPR_2019/papers/Neven_Instance_Segmentation_by_Jointly_Optimizing_Spatial_Embeddings_and_Clustering_Bandwidth_CVPR_2019_paper.pdf))
>"However, a downside to this method is that the margin $$\delta $$ has to be selected based on the smallest object, ensuring that if two small objects are next to each other, they can still be clustered into two different instances. If a dataset contains both small and big objects, this constraint negativelyinfluences the accuracy of big objects, since pixels far awayfrom the centroid will not be able to point into this small region around the centroid. Although using a hinge loss in-corporates the clustering into the loss function, given thesaid downside it is not usable in practice."

Neven et al find a unique way of integrating the clustering of pixel embeddings in an end to end learning framework. Their approach and reasoning is built in a layered, logical fashion. First the authors reason that one approach of performing pixel embedding is to predict for each pixel a displacement vector that points to the centre of the object. But this means that during the inference stage while processing a test image, in order to cluster instances, one needs the knowledge of where the centres of the objects are and how to associate the pixels with the centres. One could always resort to some density clustering and then assign pixels to the centre which minimizes the distance to the embedding. But since this clustering step is not integrated within the learning of the embeddings, it leads to inferior results.


One way of integrating clustering within a deep learning framework is to introduce a hinge loss function which depends on a margin. This margin causes the pixels far away from the centre to be embedded within a marginal sphere around the centroid of that object. But this introduces the issue that for touching small objects, the margin must be decided based on the smallest object's size. This may adversely affect results in case there are also large objects in the mix as it is a much harder task for a far away pixel of a larger object to  have a notion of where the centre is due to a limited receptive field. Hence, the authors proposed to learn a margin which they conjecture - should be large for larger objects and small for smaller objects. The authors replace the hinge loss with a gaussian function perhaps to be continuous and differentiable everywhere in the vicinity of the centre of the object.  


This still leaves the question of where the centres of the objects are. The authors propose to additionally let the network learn this by predicting for each pixel a seediness score which should be 1 if the pixel embedding coincides with the centre or centre embedding. I think this makes a lot of sense and  is a very cool work!

