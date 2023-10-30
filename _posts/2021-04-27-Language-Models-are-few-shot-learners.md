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

The authors state that humans can perform language tasks from only a few examples or from simple instructions. Previous approaches required pretraining on a large corpus of text and then fine tuning on a specific task. In contrast, trained large language models are better at few shot tasks. The figure paper taken from their paper gives a nice distinction between zero shot, one shot and few shot tasks. 
<p><figure><img src="/images/2021-04-27/few-shot.png" alt=""/></figure></p>

