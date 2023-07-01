---
layout: post
title: Learn to Pay Attention
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Jetley et al, 2018](https://arxiv.org/pdf/1804.02391.pdf))
>"One approach to visualising and interpreting the inner workings of CNNs is the attention map:  a scalar matrix representing the relative importance of layer activations at different 2D spatial locations with respect to the target task (Simonyan et al., 2013).  This notion of a nonuniform spatial distribution of relevant features being used to form a task-specific representation, and the explicit scalar representation of their relative relevance, is what we term ‘attention’"

Jetley and colleagues provide an interesting approach to add an end-to-end traininable attention module for image classification tasks. The authors premise their work by stating that for better interpretability of CNNs, one could use a scalar attention map, generated as a feature response after some activation layer of a trained network, which shows the relative importance of each spatial location for the task at hand. The authors further conjecture that one could benefit from identifying salient regions in an image and amplifying their influence while at the same time, suppressing the irrelevant and confusing information in the region. Doing this, would allow the network to be more robust to changes in data distribution (which may occur if the training and test distribution differ). The authors show that with the inclusion of the end-to-end trainable attention module, the various layers in the network learn to fixate on different important regions of the network in a better way (i.e. there is better factorisation of important image regions).

The authors include the attention module in a VGG network, they let the earlier layers have a say in the final task by concatenating the feature response of these earlier layers with the feature response obtained by the first final fully connected layer : this is akin to concatenating a local response (using a limited size receptive field as support) with a global response (using the whole input image as support).


(from [Oktay et al](https://arxiv.org/pdf/1804.03999.pdf))
>" ... this approach leads to excessive and redundant use of computational resources and model parameters; for instance, similar low-level features are repeatedly extracted by all models within the cascade. To address this general problem, we propose a simple and yet effective solution, namely attention gates (AGs).  CNN models with AGs can be trained from scratch in a standard way similar to the training of a FCN model, and AGs automatically learn to focus on the target" 

Oktay and colleagues extend this idea to propose a network architecture called as Attention U-Net for the task of biomedical-image semantic segmentation. The use an attention gate which they place at the skip connections. This allows them to combine the feature responses from the coarser layers in the encoder and the feature responses from the decoder layer. The authors show improved results for semantic segmentation on different datasets with the inclusion of these attention gates. 

I was trying to think of a simplistic way to understand the formulation of Oktay. In the vanilla form (without attention), I guess the process of concatenation of the encoded feature response with the decoded feature response through skip connections can be seen as:

$$ x + f(x) $$

where x is the feature response after the encoded layer, f represents the set ofoperations (convolutions, non-linear activations etc) until the corresponding decoding layer.

With attention, this would become somewhat like:

$$  s(x, f(x)) * x + f(x) $$

where s is the measure of similarity between the feaure response after the encoded layer and the feature response after the corresponding decoded layer (for example, this could be cosine distance). This similarity tensor which should be of the shape $ B \times 1 \times Y \times X $ (it has one channel!) weighs the feature response after the encoding layer.    
 

 
