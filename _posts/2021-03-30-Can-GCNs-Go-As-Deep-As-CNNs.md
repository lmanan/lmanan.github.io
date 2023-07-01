---
layout: post
title: Can GCNs Go as Deep as CNNs?
categories: Computer Science
tags:
mathjax: true
comments: true
---

(From [Li et al, 2019](https://arxiv.org/pdf/1904.03751.pdf))
>"A key reason behind the success of CNNs is the ability to design and reliably train very deep CNN models. In contrast, it is not yet clear how to properly train deep GCN architectures, where several works have studied their limitations. Stacking more layers into a GCN leads to the common vanishing gradient problem. This means that back-propagating through these networks causes over-smoothing, eventually leading to features of graph vertices converging to the same value. Due to these limitations, most state-of-the-art GCNs are no deeper than 4 layers."


Li et al, 2019 state that despite all the successes of CNNs, they fail with non-euclidean input data. On the other hand, while GCNs borrow concepts from CNNs and work well with non-euclidean data, most implementations (until the year 2019) were at the most four layers deep since increasing the number of layers lead to an over-smoothing of the features learnt per node. 

The authors argue that the ideas of (1) residual connections between input and output layers (as popularized by ResNets) (2) connections across layers (as introduced by DenseNet) and (3) Dilated Convolutions, can be applied to GCNs. An existing implementation by [Wang et al, 2019](https://arxiv.org/pdf/1801.07829.pdf) designed for the task of semantic segmentation on point clouds is extended to become a 56-layer GCN and the authors show an increase of close to 4% in mIoU metric. A nice project page of their work is available [here](https://www.deepgcns.org/arch). 

<p><figure><img src="/images/2021-03-30/demo.png" alt=""/></figure></p>


In most of the GCN frameworks, there are three functions which need to be learnt pertaining to message construction for vextex $v$, message aggregation and vertex update. (Here, below we only consider the case that vertex features are updated though it is easy to extend this for updating edge features).

(Message Construction)
$$
m_{vu}^{(l)} = \rho^{(l)} \left( h_{v}^{(l)}, h_{u}^{(l)}, h_{vu}^{(l)} \right)
$$

(Message Aggregation)
$$
m_{v}^{(l)} = \zeta^{(l)} \left( m_{vu}^{(l)} \right)
$$

(Vertex Update)
$$
h_{v}^{(l+1)} = \phi^{(l)} \left( m_{v}^{(l)}, h_{v}^{(l)} \right)
$$


In the [Deeper GCN](https://arxiv.org/pdf/2006.07739.pdf) publication, the authors show results on the [Open Graph Benchmark](https://ogb.stanford.edu/docs/nodeprop/) dataset which is available in PyTorch Geometric under the `ogb` module. 

To load the dataset, use:

```
from ogb.graphproppred import PygGraphPropPredDataset
from torch_geometric.data import DataLoader

dataset = PygNodePropPredDataset('ogbn-proteins', root='../data')
```

To evaluate results, use:
```
from ogb.graphproppred import Evaluator
evaluator = Evaluator('ogbn-proteins')

train_rocauc = evaluator.eval({
        'y_true': torch.cat(y_true['train'], dim=0),
        'y_pred': torch.cat(y_pred['train'], dim=0),
    })['rocauc']

valid_rocauc = evaluator.eval({
        'y_true': torch.cat(y_true['valid'], dim=0),
        'y_pred': torch.cat(y_pred['valid'], dim=0),
    })['rocauc']

test_rocauc = evaluator.eval({
        'y_true': torch.cat(y_true['test'], dim=0),
        'y_pred': torch.cat(y_pred['test'], dim=0),
    })['rocauc']

```

Some of these datasets are pretty large. For example, the protein dataset contains graphs from 8 different species, each containing 130,000 nodes. At the moment, I think the way this is handled while training is to use a batch size of 1 i.e. a single graph is loaded for each step, but the graph is partitioned into smaller sub-graphs and these several induced subgraphs together constitute a mini-batch (For example, see `https://pytorch-geometric.readthedocs.io/en/1.6.1/modules/data.html#torch_geometric.data.RandomNodeSampler`).

Note `DeepGCN` and `DeeperGCNs` are implemented in [Pytorch Geometric ](https://pytorch-geometric.readthedocs.io/en/latest/modules/nn.html) as `DeepGCN` and `GENConv`.
