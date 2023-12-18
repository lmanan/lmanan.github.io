---
layout: post
title: Preserved Neural Dynamics 
categories: Computer Vision
tags:
mathjax: true
comments: true
---


(From [Safaie et al, 2023](https://www.nature.com/articles/s41586-023-06714-0))
>"Animals of the same species exhibit similar behaviours. These behaviours are shaped at the species level by selection pressures over evolutionary time scales. Yet it remains unclear how these common behavioural adaptations emerge from the idiosyncratic neural circuitry of each individual."

Safaie et al, 2023 wonder how animals of the same species demonstrate similar behaviour. They say that similar behaviour has probably been shaped by evolution and selection pressure. But how this correlates with neural activity is not clear so far. 

>"The same behaviour performed by two individuals could be produced by preserved latent dynamics. ... We posit that preserved circuit constraints give rise to a species wide neural landscape and the individual specific latent dynamics are different instantiations of a common trajectory through this landscape."

Essentially they hypothesize that in the latent space, the dynamic trajectory seen would be the same across different individuals. Here, by latent they refer the calculated embedding which results by performing a canonical correlation analysis (CCA) on the neural recordings.  
