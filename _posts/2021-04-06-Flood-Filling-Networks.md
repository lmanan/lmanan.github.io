---
layout: post
title: Flood Filling Networks 
categories: Computer Science
tags:
mathjax: true
comments: true
---

(From [Januszewski et al, 2016](https://arxiv.org/abs/1611.00421))
>"The incorporation of supervised learning into boundary detection has been crucial to achieving increasingly accurate segmentation results. From an end-to-end machine learning point of view, however, a drawback of this pipelined approach is the fundamental disconnect between the learned part of the pipeline (pixel-wise boundary prediction) from the subsequent steps (connected components, watershed, etc) that produce the actual segments. The parameters of the boundary prediction function are optimized without consideration of the way in which individual predictions will ultimately be integrated by (an) algorithm such as the watershed into a global interpretation of the image."

Januszewski et al, 2016 propose the *Flood Filling Networks* (FFN) for performing instance segmentation of the neurites with a goal to have a synapse-level resolution. 

The authors mention that a drawback of previously existing approaches was that the task was often split into two sequential steps - a boundary prediction step where a trained model identifies the voxels belonging to the neurite (membrane) class and then a watershed/connected components/multi-cut, etc step where these pixel-level predictions are agglomerated to obtain unique instances with distinct ids. Since these would constitute as two independent steps which are not optimized jointly for the end goal of instance segmentation, inaccuracies are introduced while reconstructing neurites and attempting to identify synapses.


In *FFN*, a 3D sub-volume of the data is fed to the network which produces an object mask probability map. This 3D sub-volume of the data contains $2$ channels, one showing the gray-scale intensities of the raw image and the other showing the previous state of the object mask in the form of a probability map.  By moving the center of field of view and repeatedly performing network inference in a recurrent manner, one is able to infer the complete object mask. 

The network comprises of a deep stack of 3D convolutions with ReLU nonlininearities. No pooling is used in the network in order to obtain an output probability map with the same height, width and depth as the input sub-volume. The step size used for changing the field of view was set equal to $(8, 8, 4)$ in $(x,y,z)$. *Binary cross entropy loss* was used as the loss function which compares the binary mask predicted per object to the underlying ground truth binary mask for that object. Finally, during inference, the authors mention a heuristic strategy for selecting the initial seed points. Also the soft probability map returned by the last iteration of the recurrent network is converted into a hard probability map by thresholding at $0.9$.

When I read this paper, I wondered whether it made sense to replace the binary cross entropy with a differentiable *IoU* loss function instead maybe?! Also, I didn't fully find out yet how many iterations of the recurrent neural network are used by default (maybe it is clear when going through the code). Say if there are some data inaccuracies which cause folding etc in the raw, gray-scale data and which show as either a shadow (dark region, almost like the background) or an over-saturated patch, how are these dealt with during inference (as these mistakes would likely not be too common in the training data). Finally, I wondered whether the simple heuristic of selecting initial seed points could be replaced by something more robust.

**[This](https://www.youtube.com/watch?v=5Wr5ghHF-gk)** talk from the last author was a welcome one in sharing the complete outlook towards the connectomics project. One nice thing which the speaker introduced was the *Expected Run Length* (ERL) metric at time stamp 17:00 - if one picks a random point within a neuron, then how far can one go before the trained model makes a mistake (on an average) is quantified by this metric. 

Another cool thought was mentioned at time stamp 12:23 - here the connectivity matrix is introduced and the speaker mentions that any upstream mistake in predicting the neurite id causes a completely unreliable matrix due to percolation of errors from a localised mistake. It made me  wonder if obtaining the GT connectivity matrix should be set as the actual goal during the training phase (and hence which would update the model weights), somewhat similar in outlook to *AlphaFold* where again the goal is to predict a connectivity matrix as close as possible to the GT connectivity matrix during the training phase (the obvious difference, of course is, that in *AlphaFold* one already has access to the tokens in a sequence, while in connectomics, the *tokens* or the individual neurites first need to be segmented ...).


 
