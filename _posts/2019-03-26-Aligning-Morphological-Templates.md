---
layout: post
title: Elusive Bands
categories: Computer Vision
tags: 
mathjax: true
comments: true
---

(From Goethe)
> "Being is eternal; for laws there are to conserve the treasures of life on which the Universe draws for beauty."

The last couple of weeks have had me trying to figure out automated strategies for the alignment of *Platynereis* specimens. My end goal is to take an image of an early stage specimen and orient it such that the ventral side faces the observer while the anterior side is positioned upwards. In my trials with point cloud registration, I noticed that *initial conditions matter* and therefore I felt that this automated alignment would serve the purpose of registering coarsely (and in the process, provide a good initial condition for the fine registration which would follow later).

To begin with, I tried to incorporate some heuristics which would indicate the morphological body-axes. One of these ideas was that determining the plane of the ciliary band would allow the estimation of the direction of the anterior-posterior (A-P) axis, which sits orthogonal to the plane. This goes along the lines of what other researchers have done, except that in their case, the acetylated-tubulin signal of the ciliary band signal was quite prominent, which is not true for my imaging data. For example, [Asadulina and colleagues](https://evodevojournal.biomedcentral.com/articles/10.1186/2041-9139-3-27) quote:
>"For 48 hpf larvae, we defined the AP orientation based on the prominent acetylated-tubulin signal of the prototroch ciliary band. We subsequently found the position of the ventral nerve cord in the acetylated-tubulin signal to define the DV axis."

In order to get around the absence of availability of a strong acetylated-tubulin signal, I decided to use the DAPI-stained nuclei signal as proxy for the ciliary band. My reasoning was that by looking at the nuclei channel information, one can often make out where the ciliary band should be.  For example, the images of two fixed specimens at developmental stage 16hpf below suggest that the ciliary band lies roughly in the middle (it corresponds to the shadowy part of the image and divides the bowl of cells into the head and trunk).


<p float="center">
<img src="/images/2019-04-04/01_MovieOne.gif" width= "350" /> 
<img src="/images/2019-04-04/02_MovieTwo.gif" width ="350"/>
</p>
 



