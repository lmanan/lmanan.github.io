---
layout: post
title: Learning from Noisy Labels
categories: Computer Vision
tags:
mathjax: true
comments: true
---



(From [Zheltonozhskii et al, 2021](https://arxiv.org/abs/2103.13646))
>"C2D is motivated by the observation of an inherent obstacle that is at the core of LNL methods. It has been shown that deep networks can perform meaningful learning in the presence of noise before they enter a memorization phase. LNL methods utilize this behavior by performing a warm-up – supervised training on the full set of (noisy) labels for a short period of time. Most methods utilize either “hard” (starting an LNL procedure after a number of epochs) or “soft” (gradually increasing the weight of additional regularization terms) version of warm-up."


[Zheltonozhskii et al, 2021](https://arxiv.org/abs/2103.13646) make a nice distinction between self supervised learning (SSL) and learning with noisy labels (LNL). The former, they say, assumes a small quantity of high quality labeled data and a large quantity of unlabeled data of the same distribution. The latter, they say, assumes a large quantity of cheap annotations. The authors further draw parallels between the two domains by stating that many SSL approaches are based on predicting pseudo labels for unlabeled data, which are in effect, noisy labels. Furthermore, an LNL setting can be cast into an SSL setting by discarding noisy labels (separating noisy labels from clean ones is a key challenge in SSL).

The authors also highlight that many LNL approaches begin with a warm-up phase - which is a short supervised training on the full noisy dataset, before any sophisticated approach to deal with label noise. They claim that this phase owing to its simplicity has not received much attention perhaps, but if one were to run it for longer, then one runs into risk of the model starting to memorize the noise. So the performance of this initial phase is contingent upon the number of training iterations and other hyperparameters (learning rate etc).

The authors counter this difficulty by replacing the warm up phase by unsupervised pretraining. This is, for example, similar to the SimCLR, where initially one has a period of self-supervised pretraining and then one finetunes on a small fraction of supervised labels. The authors also specify that if one had access to a large clean label dataset (for example, ImageNet), then one could finetune on that as the first phase. But this strategy is useful only if the actual dataset has a similar distribution to the ImageNet dataset (which may not be the case say for medical image datasets). Sometimes this supervised finetuning lead to strange consequences as well which forced the authors to conclude that:

>"Our results indicate that supervised pre-training has a generally unpredictable effect on LNL. We leave the influence  analysis of different conditions (e.g. domain gap or noise level) to future work. On the contrary, using C2D resulted
in consistent improvement across all our experimental settings. In addition, C2D does not require external data nor additional supervision."

In Contrast2Divide (C2D), the authors provide a simplistic, two-step strategy. The traditional warm up phase is replaced by the Contrastive Self-Supervised Learning from SimCLR in the first step (the `Contrast` phase). This is followed by the `Divide` phase, for which the authors used a standard LNL strategy (they used`DivideMix` and `ELR+`). 
