---
layout: post
title: Structured Illumination Microscopy
categories: Computer Vision
tags:
mathjax: true
comments: true
---
(From [Gustafsson et al, 2008](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=2ahUKEwj9zreJpa3jAhVDJ1AKHUZ1BNYQFjAAegQIAxAB&url=https%3A%2F%2Fwww.ncbi.nlm.nih.gov%2Fpubmed%2F18326650&usg=AOvVaw1Px2DeLd9Ls9kxTdviK4P5))
> "The OTF support of the conventional microscope is a torus like region. Extending the resolution, in a true sense, is equivalent to finding a way to detect information from outside of this observable region. That apparently self-contradictory task is possible because the structure of interest to the microscopist is not actually the emission but rather the object structure: the density distribution of fluorescent dye. "


One of the advantages of light microscopy is its specificity in detecting certain bio-molecules. Its major weakness, though, is moderate spatial resolution - which arises on account of its wavelength. **Spatially Structured Illumination Microscopy** succeeds in alleviating this problem and doubling the lateral resolution of a fluoroscence microscope. This essay aims at explaining the underlying theory behind this technique better and draws from [Gustafsson et al](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=2ahUKEwj9zreJpa3jAhVDJ1AKHUZ1BNYQFjAAegQIAxAB&url=https%3A%2F%2Fwww.ncbi.nlm.nih.gov%2Fpubmed%2F18326650&usg=AOvVaw1Px2DeLd9Ls9kxTdviK4P5).

The resolution properties of an optical system are described by its Point Spread Function (PSF) $$H(\vec{r})$$. The observed data is a convolution of this PSF with the fluoroscent emission $$E(\vec{r})$$. 

$$
D(\vec{r}) = E(\vec{r}) \circledast H(\vec{r})
$$

The fourier transform of the PSF is called as the Optical Transfer Function and describes how strongly, and with what phase shift, the spatial frequency $$\vec{k}$$ of the object is transferred into the measured data. Due to the classical resolution limit, only a portion of the optical transfer function is able to manifest itself - this region is referred to as the *support* of the Optical Transfer Function. The resulting emission rate is directly proportional to the product of the illumination intensity $$I(\vec{r})$$ and the fluorophore density distribution $$ S(\vec{r})$$.

$$
E(\vec{r}) = S(\vec{r}) I(\vec{r})
$$

Gustafsson and colleagues claimed that the fluorophore density distribution information can be extracted if three conditions are met regarding the illumination pattern.  Firstly, the illumination pattern should be a sum of finite number of components, each of which is separable into an axial and lateral function.

$$
I(\vec{r}) = \sum_{m} I_{m}(z) J_{m} (\vec{r_{xy}})  
$$

Secondly, each of these lateral functions $$J_{m} (\vec{r_{xy}})$$ should contain only one spatial frequency. And lastly, the illumination pattern is maintained fixed in relation to the focal plane of the microscope as the several slices of the volumetric stack are imaged. 

$$
D(\vec{r}) = H(\vec{r}) \circledast E(\vec{r}) = H(\vec{r}) \circledast SI(\vec{r}) = \sum_{m} \int H(\vec{r} -\vec{r}^{'}) S(\vec{r}^{'}) I_{m}(z-z^{'})J_{m}(\vec{r}^{'}_{xy}) \mathrm{d} \vec{r^{'}} 
$$
