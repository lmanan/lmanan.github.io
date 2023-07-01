---
layout: post
title: Optical Detection of Densely Labeled Synapses   
categories: Biology 
tags:
mathjax: true
comments: true
---


(From [Mishchenko, 2010](https://www.nature.com/articles/npre.2010.4144.1.pdf))
>"EM is widely accepted to be the only tool for such reconstructions of neural connectivity with the precision of individual synapses. In this paradigm, the process of reconstruction is approached in the following way: tiny synaptic contacts are first located in the neuropil using EM; pre-synaptic axons and post synaptic dendrites are identified in the EM images for each synaptic contact; axonal and dendritic projections are traced back to their respective cell bodies using EM over macroscopically large distances. Unfortunately this paradigm suffers from two major drawbacks - the acquisition rate of electron microscopy is extremely low  and tracing of neural projections in EM images through densely packed neuropil has proven to be very difficult. Such reconstructions are also vulnerable to imaging and analysis errors, where every error in a long sequential trace of an axon can lead to devastating consequences for the entire reconstruction by causing large number of that axon's synapses, downstream of the site of error, to be lost or mislabeled. Expected frequency of such errors, unfortunately, is quite high"


Mishchenko, 2010 state that a typical way of mapping the neural connectivity optically is through EM which offers a high resolution and enables identifying the synapses and the adjacent axons and dendrites, which can be traced back to their respective cell bodies. However, the size of the synapses and the adjacent axon and dendrites have different scales - hence a mistake in tracing the axon and dendrites leads to a several downstream errors in mapping the connectivity. The authors mention the work of Lichtman et al (Brainbow) where different neurons are made to express different mixtures of fluorophones - hence each complete neuron is labeled with a different color.

The authors then state that Brainbow strategy is successful only if the neurites are not packed densely. Because of the diffraction limit, the fluorescence of different neurons, if densely packed, tends to blend in together, making individual neurites discernible. The authors conjecture that if it were possible to only label the synapses which are much more sparse than the individual neurites, then one would not run into this issue of blending of fluorescence. 


>"Assuming that synapses of different neurons could be tagged with different mixtures of fluorophores using the Brainbow construct, the fluorescence color of different synapses could be used to immediately identify the pre- and post-synaptic cells at each synaptic connection. This would allow mapping neural connectivity using optical tools, rapidly, in scalable manner, and without tracing neural projections."
