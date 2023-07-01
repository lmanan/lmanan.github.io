---
layout: post
title: Marching Cubes
categories: Computer Vision
tags:
mathjax: true
comments: true
---


(From [Lorensen and Cline](https://dl.acm.org/doi/pdf/10.1145/37402.37422), 1987)
>"Since there are eight vertices in each cube and two states, inside and outside, there are only $2^{8}$ = 256 ways a surface can intersect the cube. By enumerating these 256 cases, we create a table to look up surface-edge intersections , given the labeling of a cubes vertices. The table contains the edges intersected for each case.
Triangulating the 256 cases is possible but tedious and error - prone . Two different symmetries of the cube reduce the problem from 256 cases to 14 patterns. First, the topology of the triangulated surface is unchanged if the relationship
of the surface values to the cubes is reversed. Complementary cases, where vertices greater than the surface value are interchanged with those less than the value, are equivalent. Thus , only cases with zero to four vertices greater than the surface value need be considered, reducing the number of cases to 128. Using the second symmetry property, rotational symmetry, we reduced the problem to 14 patterns by inspection."
