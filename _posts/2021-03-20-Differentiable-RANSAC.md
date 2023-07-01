---
layout: post
title: Differentiable RANSAC
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Brachmann et al, 2018](https://arxiv.org/pdf/1611.05705.pdf))
>"In the case of RANSAC, the non-differentiable operator is the argmax operator which selects the highest scoring hypothesis. Similar to [41], we might substitute the argmax for a soft argmax, which is a weighted average of arguments [6]. We indeed explore this direction but argue that this substitution changes the underlying principle of RANSAC. Instead of learning how to select a good hypothesis, the pipeline learns a (robust) average of hypotheses. We show experimentally that this approach learns to focus on a narrow selection of hypotheses and is prone to overfitting. Alternatively, we aim to preserve the hard hypothesis selection but treat it as a probabilistic process. We call this approach DSAC – Differentiable SAmple Consensus – our new, differentiable counterpart to RANSAC. DSAC allows us to differentiate the expected loss of the pipeline w.r.t. to all learnable parameters. This technique is well known in reinforcement learning, for stochastic computation problems like policy gradient approaches."

Recently [this](https://twitter.com/eric_brachmann/status/1480889255379484677) tweet which suggested that Differentiable RANSAC is much faster than Vanilla RANSAC, made me want to understand the theory better. 

In [this](https://arxiv.org/pdf/1611.05705.pdf) work, the authors suggest that RANSAC is a building block for hypothesis selection but is often not used in deep learning pipelines on account of its non differentiability. They propose two pipelines (the latter of which is inspired by reinforcement learning) and apply this to the problem of camera localisation, where DSAC seems to help. 



