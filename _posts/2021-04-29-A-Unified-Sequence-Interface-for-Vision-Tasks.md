---
layout: post
title: A Unified Sequence Interface for Vision Tasks
categories: Computer Vision
tags:
mathjax: true
comments: true
---


(From [Chen et al, 2022](https://proceedings.neurips.cc/paper_files/paper/2022/file/cb0f9020c00fc52a9f6c9dbfacc6ac58-Paper-Conference.pdf))
>"Training a single neural network model capable of performing myriad tasks is a major step towards artificial general intelligence. In recent years, with the rise of big language models using Transformers, many different language and related tasks are unified under a single modeling framework, where a language model is trained to predict the solution given a prompt of a task description. This is only possible because these tasks - both task description and prompt can be expressed in the same, rich language interface."

Chen et al say that the vision tasks can be unified in a language interface. Only a few times, the solution is sometimes expressed in natural language (for example, image captioning tasks) but most of the tasks can not be expressed in natural language. For example, object detection is expressed in terms of bounding boxes, instance segmentation is expressed in terms of dense pixel segmentation masks.

In the pursuit of artificial general intelligence, Chen et al propose a language modeling framework for vision tasks. Since this unifies different tasks, the authors argue that this would simplify design of architectures and enable greater a degree of representation learning.

For the task of instance segmentation, the authors use a polygon representation of the contour of the mask. During training, they randomly start the sequence from any of the contour coordinates. (I wonder if they experimented with starting from the theta = 0 coordinate like in StarDist?)

They train a model for single tasks and also train a model for performing all tasks (object detection, instance segmentation, image captioning and keypoint detection) and notice that in the latter case, the model does equally well. 
