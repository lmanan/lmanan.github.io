---
layout: post
title: MOTS metrics
categories: Computer Vision
tags:
mathjax: true
comments: true
---

(From [Voigtlaender et al, 2019](https://arxiv.org/pdf/1902.03604.pdf))
>"In particular, results of recent tracking evaluations show that bounding box tracking performance is saturating. Further improvements will only be possible when moving to the pixel level. We thus propose to think of all three tasks - detection, segmentation and tracking - as interconnected problems that need to be considered together."


Voigtlaender et al distinguish different, previously available datasets and tasks for tracking - (1) multi-object tracking (MOT) datasets which include an unknown number of objects belonging to specific number of classes which need to be tracked in the evaluation RGB images. In training data, one has objects of these classes annotated as bounding boxes and with the tracklet id information. Also some of these objects may leave the scene and reappear later. These images tend to be quite dense (2) video object segmentation dataset  - here instance segmentations of one or multiple generic objects is provided in the first frame and need to be segmented with pixel accuracy and tracked in the subsequent frames. 

Contrary to these tasks, the authors propose the MOTS task where dense pixel segmentations are provided during training stages (and desired at the time of evaluation). The authors provide two datasets - MOTSChallenge and KITTIMOTS. In the former, objects of only one class (pedestrian) are annotated, while in the latter, objects of two classes (pedestrian and cars) are annotated.

This is a great contribution, in my opinion. Additionally, since all the metrics until this paper were evaluating IOU based on overlapping bounding boxes, the authors here had to define some new metrics for comparing areas of irregular objects, under the constraint that any pixel can be at most assigned to only one object (GT or prediction).


`MOTA` as defined for bounding boxes annotations (here $\vert \text{FN} \vert$ is the number of false negatives, $\vert \text{FP} \vert$ is the number of false positives, $\vert \text{IDS} \vert$ is the number of id switches and $\vert \text{M} \vert$ is the number of ground truth objects across all time frames in the sequence):

$$
\text{MOTA} = 1 - \frac{|\text{FN}| + |\text{FP}| + |\text{IDS}|}{|\text{M}|} = \frac{\vert \text{TP} \vert - \vert \text{FP}\vert - \vert \text{IDS} \vert}{\vert \text{M} \vert}
$$   

Formally, $\text{IDS}$ denote the number of ground truth masks whose predecessor was tracked with a different id. To distinguish from $\text{MOTA}$ which is evaluated using bounding boxes, the authors propose $\text{MOTSA}$.

Next, they introduce a second metric called $\tilde{\text{TP}}$. 

$$
\tilde{\text{TP}} = \sum_{h \in \text{TP}} \text{IOU} \left( h, c(h) \right)
$$

Here, $h$ indicates a prediction hypothesis $c(h)$ indicates the corresponding  GT object it maps to. $TP$ is the complete set of true positive hypotheses. If all predicted hypotheses overlap exactly with the GT masks, then $\tilde{\text{TP}}$ = $\vert \text{TP} \vert$. 

A third metric which was introduced is called $\text{MOTSP}$ which is multiobject tracking and segmentation precision:

$$
\text{MOTSP} = \frac{\tilde{\text{TP}}}{\text{TP}}
$$

The fourth metric which is somewhat similar to $\text{MOTSA}$ is $\text{SMOTSA}$

$$
\text{SMOTSA} = \frac{\tilde{\text{TP}} - \vert \text{FP}\vert - \vert \text{IDS} \vert}{\vert \text{M} \vert}
$$

I can surmise that since $\tilde{\text{TP}} < \vert \text{TP} \vert $, hence, $\text{SMOTSA}$ < $\text{MOTSA}$. But the smaller the gap, the better is the prediction quality, since $\text{MOTSA}$ only cares about hypotheses that are above 0.5 IOU wrt the GT object (so a **digital** metric) , and $SMOTSA$ looks at it in a more continuous sense. Hence the authors also state that $\text{MOTSA}$ is more indicative of **detection** and tracking quality while $\text{SMOTSA}$ is indicative of **segmentation**, detection and tracking quality.  


One last piece of the puzzle is determining what constitutes as a true positive, false positive or false negative hypothesis. Here, for each predicted hypothesis the authors find those GT objects that overlap **strictly** more than 0.5 IOU and take the one from this set of potential matching GT objects, the one that returns the highest IOU wrt the predicted hypothesis. This way, all pairwise true positives are determined. Then the ones from the prediction hypothesis set which were not assigned are considered as False positives and the ones from the GT set which were not matched to are considered false negatives. Here the thing to keep in mind is for a match to be considered, IOU should be **strictly** more than 0.5 (typically in biomedical domain, greater than equal to definition is seen more prevalent, in my opinion)

Interestingly none of these four metrics enforce lineage consistency in the case of mitosis i.e. these metrics would not ensure, per se, that a daughter cell originates from the correct parent cell (this behaviour is specific to biomedical domain and not seen in natural images, hence it makes sense that these metrics were designed to be invariant to tracklet divisions).  
