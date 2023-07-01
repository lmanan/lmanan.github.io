---
layout: post
title: Sub-graph Isomorphism
categories: Computer Vision
tags:
mathjax: true
comments: true
---

Recently while exploring strategies to plot three dimensional scientific data in Java, I came across the [Jzy3d](http://www.jzy3d.org/) package. This proved to be a good API to plot lineages of a developing Platynereis embryo in different colors, as is done [here](https://github.com/malaalam/MastodonExperiments/blob/master/src/main/java/de/mpicbg/tomancaklab/ShowLineageFigure.java). While support for exporting an animation movie was available, other features such as producing *png* images from the data were not there unfortunately.

<p float="center"><figure><a href="https://www.youtube.com/watch?v=WElktGGKS6A"><img src="/images/2019-07-13/01_Screenshot.png" alt="" width="400"></a><figcaption>
   [Visualizing the lineage tree of a *Platynereis* embryo]</figcaption></figure></p>

Another interesting but unrelated problem was regarding replacing the traditional *track scheme* display of Mastodon, so that one sees divisions along the y-axis, instead of the elapsed time. We reckoned that this would be akin to normalizing a lineage tree. Such a visualization might allow comparing two specimens and commenting on the stereotypic development of the organism, in question. 

I adopted a top down scheme [here](https://github.com/malaalam/MastodonExperiments/blob/master/src/main/java/de/mpicbg/tomancaklab/ConvertTimeToGenerations.java): crawling across each nucleus at a given time point in the original *trackscheme* panel, I determined whether it has two daughter nuclei in the next time point  (i.e. if it has undergone mitosis) and if so, copied that node to a new instance of a *ModelGraph*. The implementation looks clunky currently, one idea to improve it is to employ a recursive scheme.

<p float="center"><figure><img src="/images/2019-07-13/02_condensedLineageTree.png" alt="" ><figcaption>
   [A "condensed" lineage tree: here, only the dividing nuclei are shown. The y-axis depicts the number of generations which have elapsed, instead of the conventional display of the total time]</figcaption></figure></p>
   
Looking at the condensed form of the lineage tree made me think about the associated problem of sub-graph isomorphism. There are evidently some sub-trees which are more alike to each other than to other sub-trees (for example, **brain-1c112** and **brain-1d112**). What would be an effective algorithm and metric to quantify this similarity?
 
