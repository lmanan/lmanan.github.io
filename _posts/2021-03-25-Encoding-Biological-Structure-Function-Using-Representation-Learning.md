---
layout: post
title: Encoding Biological Structure & Function using Representation Learning
categories: Biology
tags:
mathjax: true
comments: true
---

(From [Rives et al, 2019](https://www.pnas.org/doi/10.1073/pnas.2016239118))
>"The idea that biological function and structure are recorded in the statistics of protein sequences selected through evolution has a long history. Out of the possible random perturbations to a sequence, evolution is biased toward selecting those that are consistent with fitness. The unobserved variables that determine a protein’s fitness, such as structure, function, and stability, leave a record in the distribution of observed natural sequences."

Rives et al, 2019 state that with the recent advancements in self-supervised learning and also the simultaneous explosion of protein sequences, one wonders if large scale transformer-based models can be trained to learn a representation for each of these sequences. Exploring the representation space, one hopes to find metric structure post the model training. Additionally, the resulting unsupervised representations can be investigated for the presence of biological organizing principles and information about intrinsic biological properties. 

The authors initially clarify the difference between supervised and self supervised learning - they state that self-supervised learning uses proxy tasks such as predicting the next word in a sentence given all the previous words, or predicting words that have been *masked* from their context. 

The authors explore *large* datasets such as the UniParc dataset which contains 250 million sequences, which they say is comparable to large text datasets. Next during training, the authors follow the masked language modeling objective where each input sequence is corrupted by replacing a fraction of the amino acids with a special mask token. The network is then trained to predict the missing token from the corrupted sequence. 

For each sequence x, the authors sample a set of indices M to mask, replacing the true token at each index i with the mask token. 
 
$$
L_{MLM} = E_{x \sim X} E_{M} \sum_{i \in M} - \text{log} p(x_{i} | x_{|M})
$$

Among baseline methods, the authors train two small transformer-based models - one with six layers and 42.6M parameters, and another with 12 layers with 85.1M parameters. 
