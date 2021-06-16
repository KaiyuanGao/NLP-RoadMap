

来自阿里SIGIR2021的工作，focus在推荐系统的召回，联合了I2I方式和EBR方式

- Path-based Deep Network for candidate item matching in recommenders

先来看看推荐系统常用的两种召回模式：相似度索引范式(如I2I) 和 EBR范式（如DeepMatch）。

- I2I：优点是可以有效刻画用户的多样性，且线上高效。缺点是难以建模U2I部分，从而缺乏个性化；

![NHZOiX](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/NHZOiX.png)

- EBR：优点是针对不同user会有不同表示，个性化充分，但丢失了用户兴趣的多样性

总体来说，由于受到现有召回模型框架的约束，双塔模型采用了用户信息和商品Profile信息，却无法显式地利用商品共现信息。I2I索引主要利用采用了商品共现信息，但是忽略了用户和商品Profile信息，且无法考虑行为序列对目标商品的综合影响。PDN论文就是结合两者的优势。

#### 背景

论文中将推荐问题建模为基于二度图的链式预测问题：
$$
\hat{y}_{u i}=f\left(\boldsymbol{z}_{u}, \boldsymbol{x}_{i},\left\{\boldsymbol{x}_{j}\right\},\left\{\boldsymbol{a}_{u j}\right\},\left\{\boldsymbol{c}_{j i}\right\}\right) \text { with } j \in N(u)
$$
其中，$z_u$ 是用户信息，譬如id、年龄、性别等等；$x_i$ 是目标item信息；$x_{j_k}$ 为与用户交互过的item信息； $a_{uj}$ 为用户和item的交互信息，譬如停留时长等； $c_{ji}$ 为交互item与目标item的相关性信息；

![](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/HotGmT.png)

#### PDN模型

整体框架如下，包括几个模块：Embedding layer、Trigger Net、Similarity Net、DirectNet、BiasNet。

![7yUp9U](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/7yUp9U.png)