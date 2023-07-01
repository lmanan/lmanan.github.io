---
layout: post
title: Building Probabilistic Atlases 
categories: Computer Vision
tags:
mathjax: true
comments: true
---

[Bubnis et al, 2019](https://arxiv.org/abs/1903.09227) make some interesting observations about design features while building a probabilistic atlas. They state that such an atlas should (1) maintain biological variability of features. 

>"Specifically, the atlas should maintain a probability distribution for each atlas feature or, to capture relationships between these features, a joint probability distribution over the set of atlas features."

(2) Quality of atlas should improve as new datasets are contributed to the system (3) Should produce a probabilistic output (note, this is different from (1)) and report label guesses with marginal probabilities which refect confidence of labeling

<p><figure><img src="/images/2021-03-21/fig_one.png" alt=""/><figcaption>


