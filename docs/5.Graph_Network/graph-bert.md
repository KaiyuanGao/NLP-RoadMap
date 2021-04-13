Graph-Bert：没有我attention解决不了的

## TL;DR

在这篇文章中，作者指出了目前主流GNN完全依赖图链接，存在一些严重影响性能的问题

- 模型假死(suspended animation)：随着训练层数加深，模型不再对输入数据响应；
- 过度平滑(over-smoothing)：随着训练层数加深，所有结点的embedding会越来越相似；
- 并行化处理(paralelization)：在大图上无法并行化处理

针对以上问题，提出了Graph-BERT，同样是非常火的『pretrain+fintune』范式应用于图网络中。中心思想还是attention，首先将原始大图采样为一个个的子图，然后利用attention去学习结点表证。下面来看看具体模型~

## Model

![lKHMIhyjSUcr8Tq](https://i.loli.net/2020/08/05/lKHMIhyjSUcr8Tq.png)

如上图，Graph-Bert模型主要可以分为四部分：

- 从原始大图中采样无连接子图（linkless graph）
- 输入结点embedding处理
- 基于图的transformer-encoder
- 基于图的transformer-decoder

#### 子图采样

为了更好地处理大图（并行化），graph-bert选择在采样子图上进行训练。使用了一种基于图亲密度矩阵$\mathbf{S} \in \mathbb{R}^{|\mathcal{V}| \times|\mathcal{V}|}$的采样方案：**top-k intimacy**，其中$S(i, j)$ 表示节点$v_{i}$ 和 结点$v_{j}$ 之间的亲密度，计算公式为：
$$
\mathbf{S}=\alpha \cdot(\mathbf{I}-(1-\alpha) \cdot \overline{\mathbf{A}})^{-1}
$$
其中，$\alpha$ 是一个[0, 1]之间的超参数（通常取0.15），$\overline{\mathbf{A}}=\mathbf{A} \mathbf{D}^{-1}$ 表示列规范化邻接矩阵，$A$ 为图的邻接矩阵， $D$ 为对应的对角矩阵$\mathbf{D}(i, i)=\sum_{j} \mathbf{A}(i, j)$。

那么对于一个给定的目标结点，就可以利用上面定义的亲密度来找出其上下文结点，计算公式为：
$$
\Gamma\left(v_{i}\right)=\left\{v_{j} \mid v_{j} \in \mathcal{V} \backslash\left\{v_{i}\right\} \wedge\right. \left.\mathbf{S}(i, j) \geq \theta_{i}\right\}
$$
其实这一步就是把图结构的数据转变成了我们平时常见的NLP序列化输入，把每个结点看成是一个个字或词，然后后面就可以直接套transformer了。记得之前有篇文章说的也是类似的：[Transformers are Graph Neural Networks](https://graphdeeplearning.github.io/post/transformers-are-gnns/)

#### 结点embedding

由于经过采样出来的结点们是无序的，这里按照**与target node的亲密度打分**来对结点集合进行排序。结点emdedding由四部分组成

###### 原始特征embedding

就是使用一个映射操作将原始特征表示到新的共享的特征空间，对于不同的输入可以有不同的映射函数，如CNN/LSTM/BERT等
$$
\mathbf{e}_{j}^{(x)}=\operatorname{Embed}\left(\mathbf{x}_{j}\right) \in \mathbb{R}^{d_{h} \times 1}
$$

###### Weisfeiler-Lehman 绝对角色embedding

Weisfeiler-Lehman算法是用来确定两个图是否是同构的，其基本思路是通过迭代式地聚合邻居节点的信息来判断当前中心节点的独立性(Identity)，从而更新整张图的编码表示。
$$
\begin{aligned}
\mathbf{e}_{j}^{(r)} &=\text { Position-Embed }\left(\mathbf{W} \mathbf{L}\left(v_{j}\right)\right) \\
&=\left[\sin \left(\frac{\mathbf{W} \mathbf{L}\left(v_{j}\right)}{10000^{\frac{2 l}{d_{h}}}}\right), \cos \left(\frac{\mathbf{W} \mathbf{L}\left(v_{j}\right)}{10000^{\frac{2 l+1}{d_{h}}}}\right)\right]_{l=0}^{\left\lfloor\frac{d_{h}}{2}\right\rfloor}
\end{aligned}
$$
有关更多WL算法的细节可以参考这个slides：[Graph Kernel](https://www.slideshare.net/pratikshukla11/graph-kernelpdf)

###### 基于亲密度的相对位置embedding

上一节计算的嵌入可以表示全局的信息，而这一步主要是获取局部信息。
$$
\mathbf{e}_{j}^{(p)}=\text { Position-Embed }\left(\mathrm{P}\left(v_{j}\right)\right) \in \mathbb{R}^{d_{h} \times 1}
$$

###### 基于相对距离embedding

对两个结点在原始大图中的距离（间隔边的数量）进行embedding表示，主要是为了平衡上述两步的embedding
$$
\mathbf{e}_{j}^{(d)}=\text { Position-Embed }\left(\mathrm{H}\left(v_{j} ; v_{i}\right)\right) \in \mathbb{R}^{d_{h} \times 1}
$$

#### Transfomer编码器

##### 输入

首先是对前面得到的四个embeeding进行聚合，
$$
\mathbf{h}_{j}^{(0)}=\operatorname{Aggregate}\left(\mathbf{e}_{j}^{(x)}, \mathbf{e}_{j}^{(r)}, \mathbf{e}_{j}^{(p)}, \mathbf{e}_{j}^{(d)}\right)
$$
聚合的函数有很多可以选择，文章里作者就使用了最简单的加和操作。聚合之后就可以得到所有结点的输入表示：$H^{(0)} = \left[\mathbf{h}_{i}^{(0)}, \mathbf{h}_{i 1}^{(0)}, \cdots, \mathbf{h}_{i k}^{(0)}\right]^{\top} \in \mathbb{R}^{(k+1) \times d_{h}}$

##### 更新

然后就是进行N层的transformer encoder的迭代更新，
$$
\begin{aligned}
\mathbf{H}^{(l)} &=\text { G-Transformer }\left(\mathbf{H}^{(l-1)}\right) \\
&=\operatorname{softmax}\left(\frac{\mathbf{Q} \mathbf{K}^{\top}}{\sqrt{d_{h}}}\right) \mathbf{V}+\mathbf{G}-\operatorname{Res}\left(\mathbf{H}^{(l-1)}, \mathbf{X}_{i}\right)
\end{aligned}
$$

##### 输出

经过D层的编码之后，我们就可以得到对应每个结点的表示，$H^{(D)} = \left[\mathbf{h}_{i}^{(D)}, \mathbf{h}_{i 1}^{(D)}, \cdots, \mathbf{h}_{i k}^{(D)}\right]^{\top}$ ，之后就可以根据具体的下游任务来使用这些向量表示。

## 预训练

图神经网络的预训练大致分为两部分：对结点的预训练以及对链接关系的预训练。

#### 结点预训练

对于目标结点$v_{i}$ ，原始特征为$x_{i}$，我们通过GRAPH-BERT编码层可以得到其隐藏表示$z_{i}$， 然后经过一层FC映射后$\hat{\mathbf{x}}_{i}=\mathrm{F} \mathrm{C}\left(\mathbf{z}_{i}\right)$重建原始特征。
$$
\ell_{1}=\frac{1}{|\mathcal{V}|} \sum_{v_{i} \in \mathcal{V}}\left\|\mathbf{x}_{i}-\hat{\mathbf{x}}_{i}\right\|_{2}
$$

#### 链接关系预训练

这一部分其实是考虑了图的结构信息，利用两个节点表示的cos距离与之前提前算好的亲密度打分，来做损失。
$$
\ell_{2}=\frac{1}{|\mathcal{V}|^{2}}\|\mathbf{S}-\hat{\mathbf{S}}\|_{F}^{2}
$$
其中，$\hat{\mathbf{S}}(i, j)=\hat{s}_{i, j}$

