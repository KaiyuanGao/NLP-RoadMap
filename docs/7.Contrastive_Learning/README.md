## 关于对比学习

对比学习(Contrastive Learning，以下用CL指代) 最近也非常火，属于无监督或者自监督的学习范式，不受限于标注数据，可以充分挖掘数据样本中的信息。

对比学习的主要思想也比较简单：通过自动构造相似实例和不相似实例，学习一个表示模型，通过这个模型，使得相似的实例在投影空间中比较接近，而不相似的实例在投影空间中距离比较远。

目前针对对比学习的研究点主要在：相似样本的构造、负样本的构造、表示模型的选型设计以及如何防止模型坍塌(Model Collapse)等等。本部分用于记录关注相关进展，keep reading！



#### 综述

- [A Survey on Contrastive Self-supervised Learning](https://arxiv.org/abs/2011.00362)
- [Contrastive Representation Learning: A Framework and Review](https://arxiv.org/abs/2010.05113)

#### CL + CV

#### CL + NLP

#### CL + Graph

