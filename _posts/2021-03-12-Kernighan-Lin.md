---
layout: post
title: Kernighan-Lin
categories: Computer Vision
tags:
mathjax: true
comments: true
---




[Kernighan-Lin](https://ieeexplore.ieee.org/document/6771089) Algorithm is a heuristic algorithm for partitioning graphs with costs on the edges, into subsets no larger than a given size such that the total cost on the edges being cut is minimized. 

>"One important practical example of this problem is placing the components of an electronic circuit onto printed circuit board sor substrates, so as to minimize the number of connections between the cards. The components are the nodes of the graph, and the circuit connections are the edges. There is some maximum number of components which may be placed on any card. Since connections between cards have high cost compared to connections within a board, the object is to minimize the number of interconnections between cards"

What is important to keep in mind is that the cost that has to be minimized is the summation over $c_{i,j}$ such that i and j are in different subsets. The cost is thus the external cost in the partition, as the authors put it. 

Another interesting remark which the authors make is that minimizing this external cost is equivalent to maximising the internal costs because the total costs of all edges is constant (which I thought was a cool argument!). And also of course, just by changing the sign of costs, one could instead go for maximising the external costs, which I guess is a trivial extension, but has bearing on which optimization technique one wishes to use.

The authors then go on to specify that evaluating the exact solution would be impractical. They provide a simple illustrative example: if graph G has $n$ nodes and one would like to extract $k$ subsets of size $p$ such that $k \times p = n$, then one could extract the first subset in $$ C^{n}_{p} $$ ways, the second subset in $$ C^{n-p}_{p} $$ ways and so on.

This is equivalent to
$$
\frac{1}{k!} C^{n}_{p} C^{n-p}_{p} ... C^{2p}_{p} C^{p}_{p}
$$

for simple values such as $n=10$ , $p = 4$, there are $10^{20}$ solutions.

Next the authors state that any approach which has computational complexity beyond $O(n^{2})$ would be undesirable. In the light of this, they discuss heuristic solutions which may address this challenge.

The authors reference the Ford and Fulkerson's Max Flow Min Cut approach which essentially considers the edge costs as maximum flow capacities between a pair of nodes. A cut is a separation of the set of nodes in two disjoint subsets. While elegant, the authors state that a) the theorem only allows for partitioning a graph into two subsets and b) there is no clean way to constrain the size of the subset.

The authors introduce the case of two way uniform partition. Here a set has to be partitioned into two disjoint subsets of equal size. The strategy is to start with any random partition A, B of S (set of $2n$ points). Next we try to decrease the external cost by a series of exchanges between A nd B. When no further improvement is possible, the resulting partition A', B' are locallly minimal wrt algorithm. 

So then how should one identify  the subsets from the partition A and B, without considering all possible choices. The procedure is as follows:

For each $a \in A$, we define an external cost:

$$
E_{a} = \sum_{y \in B} c_{ay}
$$

and an internal cost

$$
I_{a} = \sum_{z \in A} c_{az}
$$

We do the same for each b \in B and get $E_{b}$ and $I_{b}$.
Now the cost of exchanging any $a$ and $b$ is precisely known as:

$$ D_{a} + D_{b} - 2 c_{ab}
$$

We identify the $a_{1}$ and $b_{1}$ which give the maximum gain and put them aside. Next we repeat the procedure for all elements other than $a_{1}$ and $b_{1}$ and identify the next sequence of elements which must be exchanged. We repeat the process for $k$ steps such that the gain $g_{1} + g_{2} + ... + g_{k}$ is maximised. 

The authors also explained how to extend this approach to non-equal partitions and more than two partitions. 


[Here](https://github.com/hci-unihd/plant-seg/blob/8428b1f05ca02398c4badb4a99001c4031ecf8a4/plantseg/segmentation/multicut.py#L65) is where the Kernighan Lin algorithm was used to obtain instance segmentations in `PlantSeg`. Maxima of distance transform on predicted boundary maps were used as seeds to first run seeded watershed. This gives some supervoxels which are used to construct a region adjacency graph. Next the edge weights between the super voxels are computed as the mean score of the membrane probability map along these vectors (if an actual nucleus is oversegmented, then the mean edge weight is quite low since it never crosses any boundary but if the edge is placed between two actual objects, then the edge weight is high. These edge weights are first converted to costs and next the multicut approach using Kernighan-Lin is applied):

``` 
def segment_volume(self, pmaps):
        if self.ws_2D:
            # WS in 2D
            ws = self.ws_dt_2D(pmaps)
        else:
            # WS in 3D
            ws, _ = distance_transform_watershed(pmaps, self.ws_threshold,
                                                 self.ws_sigma,
                                                 sigma_weights=self.ws_w_sigma,
                                                 min_size=self.ws_minsize)

        rag = compute_rag(ws)
        # Computing edge features
        features = nrag.accumulateEdgeMeanAndLength(rag, pmaps, numberOfThreads=1)  # DO NOT CHANGE numberOfThreads
        probs = features[:, 0]  # mean edge prob
        edge_sizes = features[:, 1]
        # Prob -> edge costs
        costs = transform_probabilities_to_costs(probs, edge_sizes=edge_sizes, beta=self.beta)
        # Creating graph
        graph = nifty.graph.undirectedGraph(rag.numberOfNodes)
        graph.insertEdges(rag.uvIds())
        # Solving Multicut

        node_labels = multicut_kernighan_lin(graph, costs)
        return nifty.tools.take(node_labels, ws)
```

What confuses me somewhat is there is no specification of the max size of each of these partitions while running kernighan-lin (I thought this was one of the required parameters).  
