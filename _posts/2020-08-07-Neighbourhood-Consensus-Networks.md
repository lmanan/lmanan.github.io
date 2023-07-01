---
layout: post
title: Neighbourhood Consensus Networks 
categories: Computer Vision
tags:
mathjax: true
comments: true
---
(From [Rocco et al](https://arxiv.org/pdf/1810.10510.pdf))
>"Determining the correct matches from the correlation map is, a priori, a significant challenge. Note that the number of correct matches are of order of hw, while the size of the correlation map is of the order of (hw)2.  This means that the great majority of the information in the correlation map corresponds to matching noise due to incorrectly matched features"

[Rocco and colleagues](https://www.di.ens.fr/willow/research/ncnet/) state that the problem of graph matching is made hard because of appearance differences and also because of repeating textures. They argue that in presence of repeating textures or pattern, the only way to disambiguate a match on a repetitive pattern is to analyze a larger context of the scene that contains a unique, non-repeating pattern. The information from this can be propagated to neighboring uncertain matches. Or that these unique matches will *support* the closeby uncertain matches, in the authors' words. The authors hark back to the related idea of *neighborhood consensus* or including *semi-local* constraints. Traditionally, the neighborhood consensus used to be applied as a filtering step after performing a hard assignment on a variant of nearest neighbour by using Euclidean distance in the feature space.  But the authors correctly judge that in certain scenarios, the correspondences are not the means to an end -result of alignment/registration but rather the desired end result themselves, and hence following up the filtering after performing the hard assignment only provides a subset of correspondences, which is less than a desired objective. 

So in this work, the authors make two important changes: one, they learn the neighborhood consensus constraints directly from the training data and two, they perform the filtering *before* the hard assignment step. As contributions, the authors show that the method performs robustly in the presence of weak supervision . This way without having access to complete geometric model, the authors show high quality dense correspondences.

The proposed pipeline consists of 5 steps: (i) dense feature extraction and matching (ii) the neighbourhood consensus network (iii) a soft mutual neighbour filtering (iv) extraction of correspodences from the 4D filtered match tensor (v) weakly supervised training loss.

### Dense feature extraction and matching

Given an image, the feature extractor will produce a dense set of descriptors $$f_{ij} \in R^{d}$$ with indices i = 1 ... h and j = 1 ... w, where d is the dimensionality of the features. (I imagine that h and w is the dimensionality of the image in the bottleneck layer? Not sure!) In order to have a completely end-to-end, differentiable method, the authors store all the descriptors and compute a cosine distance between all possible pairs. This gives a 4D tensor which the authors refer to as the correlation map and indicates how strongly a feature coming from the first image correlates to a feature coming from the second image. 
(Why is cosine distance generally preferred over euclidean distance while comparing latent space features? Any advantages? Not sure! Also how is the network architecture - do they follow a downsampling path like the U-net?)

### Neighbourhood consensus network

In order to further process the correlation map, the authors use a 4D CNN. Now here, the authors make some solid arguments in my opinion:

>"We can expect correct matches to have a coherent set of supporting matches surrounding them in the 4-D space. These geometric patterns are equivariant with translations in the input images; that is, if the images are translated, the matching pattern is also translated in the 4-D space by an equal amount. This property motivates the use of 4-D convolutions for processing the correlation map as the same operations should be performed regardless of the location in the 4-D space. This is analogous to the motivation for using 2-D convolutions to process individual images – it makes sense to use convolutions, instead of for example a fully connected layer, in order to profit from weight sharing and keep the numberof trainable parameters low. Furthermore, it facilitates sample-efficient training as a single training example provides many error signals to the convolutional weights, since the same weights are applied at all positions of the correlation map.  Finally, by processing matches with a 4D convolutional network we establish a strong locality prior on the relationships between the matches. 
"

Basically, the authors state that if the corresponding patterns in an image are translated then that translation would also be captured by the correlation map: thus expressing equivariance. This property motivates the use of a 4D convolution instead of a fully connected network. Also, because of the finite basis to a convolutional kernel, the kernel would determine the quality of a match by investigating information in a local 2D neighbourhood. The final output from this component in the pipeline is a filtered correlation map where inconsistent local patterns are downweighted or removed (does that mean, set to zero? Not sure!)

### Soft mutual nearest neighbor filtering

The previous step does some suppression and amplification of the values of the correlation map, but it can not enforce constraints on a global level such as ensuring that the optimal match for any feature in image A is realy the nearest neighbor when all possible features from image B are considered. This is made possible through a soft max operation. 

...

While this appears like an exciting and well written piece of work, I wonder how one can extend this to a volumetric setup, since there one would require 6D convolutions for the neighbourhood consensus network. Also, it seems that supervision was performed at the level of image pairs: that is frankly amazing that it works so well. I do not understand why the authors did not also suggest using supervision from a sparse set of correspondences for weak supervision.    

  
 On this last front, the authors state that 
>"Obtaining such exhaustive ground-truth is complicated – dense manual annotation is impractical, while sparse annotation followed by an automatic densification technique typically results in imprecise and erroneous training data."

Why is the densification needed? Say if the ground truth correlation map is available for a few features, then one should be able to accrue differences at those locations, right? Not clear!

