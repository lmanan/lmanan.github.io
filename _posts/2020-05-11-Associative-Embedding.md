---
layout: post
title: Associative Embedding
categories: Computer Vision
tags:
mathjax: true
comments: true
---

<p><figure><img src="/images/2020-05-11/associativeEmbedding.png" alt=""/><figcaption>
[Source: Newell et al, 2017. Both multi-person pose estimation and instance segmentation are examples of computer vision tasks that require detection of visual elements (joints of the body or pixels belonging to a semantic class) and grouping of these elements (as poses or individual object instances)]</figcaption></figure></p>

(From [Newell et al](https://arxiv.org/pdf/1611.05424.pdf), 2017)
>"To train a network to predict the tags, we use a loss function that encourages pairs of tags to have similar values if the corresponding detections belong to the same group in the ground truth or dissimilar values otherwise. It is important to note that we have no “ground truth” tags for the network to predict, because what matters is not the particular tag values, only the differences between them.  The network has the freedom to decide on the tag values as long as they agree with the ground truth grouping."

Newell et al came up with an interesting strategy to perform instance segmentation. Although originally geared to address the problem of multiperson pose estimation, the authors say that the principle should also be applicable for semantic instance segmentation. The basic idea is to introduce for each pixel, a real number that serves as a `tag` to identify the group the pixel belongs to. The authors impose a loss that encourages the tags to be similar within an object instance and different across instances. The authors further state that there is no reason to do this over all possible pixel pairs - rather a small fraction of pixels can be sampled from each object instance and one could do pairwise comparisons across the group of sampled pixels.

Let $$x$$ denote a pixel location and $$h(x)$$ be the tag at that location. $$S_{n} = x_{kn}, k = 1, 2, \ldots, K $$ is the set of locations randomly sampled from the $$n^{th}$$ object istance. The grouping loss $L_{g}$ is defined as:

$$ L_{g} = \sum_{n} \sum_{x \in S_{n}} \sum_{x^{`} in S_{n}} \left( h(x) - h(x^{'}) \right)^{2} + \sum_{n} \sum_{n^{'}} \sum_{x \in S_{n}} \sum_{x^{'} \in S_{n^{'}}} \exp  \frac{-(h(x) - h(x^{'}))^{2}}{2 \sigma^{2}} $$

Some other publications also address a similar idea. Noteable ones include [Brebandre and Neven et al](https://arxiv.org/pdf/1708.02551.pdf%20http://arxiv.org/abs/1708.02551.pdf) and [Fathi et al](https://arxiv.org/pdf/1703.10277.pdf).

Although the idea seems quite compelling and quite easy to implement, I wonder that since the predicted tags could be any real number, does it make it harder for a network to generalize on an unseen image, since there is no real anchor to what the tag value should be?! Also, is there any specific reason to go for a $L_{2}$ type grouping loss, would an $L_{1}$ loss also not work? 
