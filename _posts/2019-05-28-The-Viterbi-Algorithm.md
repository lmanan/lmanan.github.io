---
layout: post
title: The Viterbi Algorithm 
categories: Computer Vision
tags: 
mathjax: true
comments: true
---

(From [Quach and Farooq, 1994](https://ieeexplore.ieee.org/document/410918))
> "Under the hypothesis tree approach similar to the approach used by the MHT algorithm to solve the data association problem as posed in section, the average number of possible tracks generated from the cumulative measurement set is (m+1)^T, where m is the average number of measurements per scan and T is the number of scans in the tracking period. For a typical tracking problem, the number of scans T is usually very large. For example, in our simulation we have 160 scans; if we have an average measurement of 3 measurements per scan, the number of feasible track configurations is 4^160 ... On the other hand, the worst case complexity of the Viterbi Algorithm can be shown to be O(T(m+1)^2)(roughly 160 X 16  = 2560 comparisions, which is significantly less than the MHT approach.)"

<div
  style="float: right; display: inline-block; border: 2px solid gray; margin: 20px; margin-right: 50px;"
  class="itk-vtk-viewer"
  data-url="https://data.kitware.com/api/v1/file/564a65d58d777f7522dbfb61/download/data.nrrd"
  data-viewport="450x300"
></div>
<script type="text/javascript" src="https://unpkg.io/itk-vtk-viewer/dist/itkVtkViewerCDN.js"></script>

<!--
<canvas id="myCanvas" width="300" height="150" style="border:1px solid #d3d3d3;">
Your browser does not support the HTML5 canvas tag.</canvas>
<script src ="{{ "/assets/underscore.js" | prepend: site.baseurl}}"></script>
-->

<!--
<script src="http://threejs.org/build/three.min.js"></script>
<canvas id="canvas" width="300" height="150" style="border:1px solid #d3d3d3;">
Your browser does not support the HTML5 canvas tag.</canvas>

<script src ="{{ "/assets/underscore2.js" | prepend: site.baseurl}}"></script>
-->

