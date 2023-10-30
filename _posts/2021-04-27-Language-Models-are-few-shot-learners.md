---
layout: post
title: LLMs are few shot learners
categories: Computer Vision
tags:
mathjax: true
comments: true
---

From [Brown et al, 2020](https://arxiv.org/abs/2005.14165)
>"Here we show that scaling up language models greatly improves task-agnostic, few-shot performance, sometimes even reaching competitiveness with prior state of the art fine-tuning approaches. Specifically, we train GPT-3, an autoregressive language model with 175 billion parameters, 10x more than any previous non-sparse language model, and test its performance in the few-shot setting."

The authors state that humans can perform language tasks from only a few examples or from simple instructions. Previous DL approaches required pretraining a language model on a large corpus of text and then fine tuning on a specific task. 
In contrast, the authors found that trained large language models are better at few-shot tasks and their performance, at times, rivals that of fine-tuning. 

From [Kirillov et al](https://arxiv.org/pdf/2304.02643.pdf) 

>"When scaled and trained with abundant corpora from the web, these models' zero and few shot performance compares well to fine-tuned models. Empirical trends show this behaviour improving with model scale, dataset size and total training compute."

The figure paper taken from Brown *et al* gives a nice distinction between zero shot, one shot and few shot tasks. 
<p><figure><img src="/images/2021-04-27/few-shot.png" alt=""/></figure></p>


