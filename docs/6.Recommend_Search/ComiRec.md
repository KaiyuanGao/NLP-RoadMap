阿里KDD 2020 的论文，和他们之前的一篇MIND比较相似，都是focus在推荐系统的多兴趣召回，

- 论文：Controllable Multi-Interest Framework for Recommendation

论文将推荐看成是一个序列预测问题，即根据用户历史交互行为，预测下一个用户感兴趣的item然后推荐给他。这样一来，如何生成一个精准的user-embedding变得十分关键，以往的工作针对一个user都只生成一个emb，然而实际上即使同一user，也会存在多种兴趣点，单一的emb无法精准全面刻画。

![sJ3q1H](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/sJ3q1H.png)

上图是论文中提出的推荐系统工作模式。

#### 模型架构

![yxS9uT](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/yxS9uT.png)

如上，ComiRec模型输入为用户的历史行为序列（item ids）以及真实购买的item作为label，经过一层embedding会得到item embedding。接着 `Multi-Interest Extraction` 会对item embedding序列进行encode得到几个`user embedding`。然后

- 训练阶段，user embdding们和真实item embedding做sampled softmax
- serving阶段，每个user embdding会通过ANN召回top-N个相似item，经过`Aggregation`层选出最终的top-K

#### Multi-Interest Extraction

MIE模块的作用在上一节介绍过，具体实现有两种形式：动态路由（Dynamic Routing）和 自注意力（Self-Attention）

##### 动态路由

与MIND论文里的比较相似，但也有一些区别。后续会列一下对比。

![ngDcY7](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/ngDcY7.png)

##### 自注意力

1. 计算attention weights

$$
\mathbf{a}=\operatorname{softmax}\left(\mathbf{w}_{2}^{\top} \tanh \left(\mathbf{W}_{1} \mathbf{H}\right)\right)^{\top}
$$

2. 计算weighted sum

$$
\mathbf{v}_{u}=\mathbf{H a}
$$

上述只是用户的单个兴趣点，扩展到K个兴趣，只需要修改下参数矩阵，公式变为，
$$
\mathbf{A}=\operatorname{softmax}\left(\mathbf{W}_{2}^{\top} \tanh \left(\mathbf{W}_{1} \mathbf{H}\right)\right)^{\top}
$$

$$
\mathbf{V}_{u}=\mathbf{H A}
$$

#### 模型训练

常规操作：sampled softmax + 交叉熵
$$
\mathbf{v}_{u}=\mathbf{V}_{u}\left[:, \operatorname{argmax}\left(\mathbf{V}_{u}^{\top} \mathbf{e}_{i}\right)\right]
$$

$$
P_{\theta}(i \mid u)=\frac{\exp \left(\mathbf{v}_{u}^{\top} \mathbf{e}_{i}\right)}{\sum_{k \in I} \exp \left(\mathbf{v}_{u}^{\top} \mathbf{e}_{k}\right)}
$$

$$
\operatorname{loss}=\sum_{u \in \mathcal{U}} \sum_{i \in I_{u}}-\log P_{\theta}(i \mid u)
$$

#### Aggregation module

标题里的`Controllabel` 就是在这里体现，目的是为了平衡accuracy和diversity。论文中的做法是：
$$
Q(u, \mathcal{S})=\sum_{i \in \mathcal{S}} f(u, i)+\lambda \sum_{i \in \mathcal{S}} \sum_{j \in \mathcal{S}} g(i, j)
$$
其中，等式右侧第一项是考虑accuracy，第二项是考虑divesity
$$
f(u, i)=\max _{1 \leq k \leq K}\left(\mathbf{e}_{i}^{\top} \mathbf{v}_{u}^{(k)}\right)
$$

$$
g(i, j)=\delta(\operatorname{CATE}(i) \neq \operatorname{CATE}(j))
$$

#### 模型上线

线上部署的时候，对每个user兴趣向量，使用ANN检索除item，然后通过聚合模块可控地选择最终top-k个item。有种多路召回的感觉。

#### 一些对比

读完模型，发现还是有很多前辈模型的影子

- **MIND**：不仔细看还真是一模一样。
  - 不同点一：ComiRec使用的是原始的CapsNet里面的动态路由方法，而MIND则是稍作了修改，基于行为转化兴趣（B2I）新的动态路由。
  - 不同点二：ComiRec最上层有一个聚合模块，实现了可控
- **MINE**：多兴趣排序模型。

