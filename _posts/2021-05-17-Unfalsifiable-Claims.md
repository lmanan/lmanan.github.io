---
layout: post
title: Unfalsifiable Claims 
categories: Computer Vision
tags:
mathjax: true
comments: true
---

From [Christensen et al, 2022](https://arxiv.org/pdf/2209.00495.pdf)



>"We focus on identifying narratives in fine-grained topic specific discussions"

Such as reddit posts? Why focus on fine grained discussions? What is the definition of fine grained, how do you decide?

>"There are a number of reasons why it is difficult to label narratives present in tweets - doing so requires insight into the topic for which the set of potentially relevant narratives is not known beforehand, the appropriate level of label granularity is not obvious and the number of label topics varies"

>"Our work can be considered an instance of fine grained topic modeling adjacent to fact checking workflows"

If you do have supervision of GT for fact checking, does that help in generating clusters which are false and non false.

What about ranking of falsehoodness in the triplet sense?
Most in between statements are 90 % true and just a little falsehood crept in it - what bout ranking those ...

>"We only focus on narratives that have gone viral, not finding the cause behind surpassing a certain virality threshold, how they were shared or the affective state in which people might view them."

Is it possible to identify the source tweet that sparked a trend eventually? Would the dynamics of virality be explainable by looking at the embedding space?
In general, it is known which tweet became the most viral, which were the secondary tweets that became less popular, i.e. the statistics of how they were shared can be used for supervision for the purpose of ranking ?
Also whoever retweets specifies that they are similar to the original tweet, so you get a lot of GT annotated data, no?

>"Our approach iteratively applies SNaCK to learn a low dimensional representation of these comments."

How is the low dimensional representation obtained? On what dataset was the SnACK model trained?

>"To study this task, we present PAPYER, a dataset based on hand drying in public restrooms suitable to study and evaluate methods for narrative discovery." 

What other applications are you looking at at the moment? 
For example, in industries it is clear to the marketing team what words to use to advertise a product. Here the number of labels are already known? DO you have companies as your clients?
Do you have significant interest from the government or from the academic bodies?



