---
layout: post
title: Traveling Salesman Problem
categories: Biology
tags:
mathjax: true
comments: true
---

Recently, looking at this [xkcd](https://xkcd.com/399/) comic got me thinking about how far machine learning has progressed in solving the routing problem.

[Bengio et al, 2020](https://arxiv.org/pdf/1811.06128.pdf) address similar NP-hard problems in a review related to how far machine learning has come in integrating combinatorial optimization (CO) as part of the model training. They initially define the problem statement of TSP as one where we are searching for a cyle of minimum length where each node is visited only once. The authors say that in the particular case of Euclidean TSP, when all the nodes are assigned coordinates in a plane and cost on each edge is the euclidean distance between two nodes, a good approximate solution can be found by leveraging the structure of the graph. 

(From [Bengio et al, 2020](https://arxiv.org/pdf/1811.06128.pdf))
>"From the CO point of view, machine learning can help improve an algorithm on a distribution of problem instances in two ways. On the one side, the researcher assumes expert knowledge about the optimization algorithm,
but wants to replace some heavy computations by a fast approximation. Learning can be used to build such approximations in a generic way, i.e., without the need to derive new explicit algorithms. On the other side, expert knowledge may not be sufficient and some algorithmic decisions may be unsatisfactory. The goal is therefore to explore the space of these decisions, and learn out of this experience the best performing behavior (policy), hopefully improving on the state of the art. Even though ML is approximate, we will demonstrate through the examples surveyed in this paper that this does not systematically mean that incorporating learning will compromise over-all theoretical guarantees. From the point of view of using ML to tackle a combinatorial problem, CO can decompose the problem into smaller, hopefully simpler, learning tasks. The CO structure therefore acts as a relevant prior for the model."

The authors state that from the point of view of CO, the benefits of ML are two fold: one, that it can help approximate certain *known* heavy computations by fast ones, and two, if some of these computations are not known, then it could enable exploring the space of algorithmic decisions.

On the other hand, an alternate approach which **does not make any concessions** on the CO side is proposed by [Vlastelica et al, 2020](https://arxiv.org/pdf/1912.02175.pdf) and their github [repository](https://github.com/martius-lab/blackbox-backprop). The authors argue that a hybrid fusion of DL with CO can not handle differentiability of combinatorial components and hence implementations, so far, opt for approximate solutions which are sub-optimal.

(From [Vlastelica et al, 2020](https://arxiv.org/pdf/1912.02175.pdf))
>"The fundamental problem with constructing hybrid architectures is differentiability of the combinatorial components. State-of-the-art approaches pursue the following paradigm: introduce suitable approximations or modifications of the objective function or of a baseline algorithm that eventually yield a differentiable computation. The resulting algorithms are often sub-optimal in terms of runtime, performance and optimality guarantees when compared to their unmodified counterparts. While the sources of sub-optimality vary from example to example, there is a common theme: any differentiable algorithm in particular outputs continuous values and as such it solves a relaxation of the original problem. It is well-known in combinatorial optimization theory that even strong and practical convex relaxations induce lower bounds on the approximation ratio for large classes of
problems (Raghavendra, 2008; Thapper & ˇZivn ́y, 2017) which makes them inherently sub-optimal. This inability to incorporate the best implementations of the best algorithms is unsatisfactory."

As the lead author mentions in [this](https://www.youtube.com/watch?v=NprYXA8VN28) nice tutorial video, the main idea is inspired by [OptNet](https://arxiv.org/pdf/1703.00443.pdf) where the authors implemented an internal layer which optimizes for a sub-problem (different from the actual problem). A nice tutorial is provided in [this](https://locuslab.github.io/2019-10-28-cvxpylayers/) notebook by the OptNet and extensions' authors. 
