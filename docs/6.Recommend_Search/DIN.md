来自阿里的2018年的论文，focus在推荐广告的CTR预估，

- Deep Interest Network for Click-Through Rate Prediction

论文面对的场景很好理解，就是在用户购物的同时推荐广告，而广告可以理解为商品，所以也是一个经典的推荐问题。

![4feSAY](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/4feSAY.png)

我们知道，推荐场景最重要的就是用户历史行为啦。在这之前的做法是，对所有的历史用户行为平等对待，即对所有交互过的历史商品embedding做average pooling。这样的做法导致的问题是，没有考虑用户的多兴趣特性，或者说在针对某一特定商品的推荐时引入了不应该的噪声信息（譬如上图中，最终推荐的是一件女士大衣，这种情况下如果将为男友买的鞋子杯子与衣服裤子同等对待，就是有问题的了）。

那么为了解决这个问题，论文里引入了attention机制，提出 `DIN`  模型框架。

#### 输入

输入包括四大类别特征：

- 用户画像特征：年龄、性别等
- 用户行为特征：其实是一系列与用户交互过的商品，特征包括商品id、店铺id、类别id等
- 广告特征：商品id、店铺id、类别id等
- 上下文特征：pid、时间等

![sbzRqc](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/sbzRqc.png)

#### Base Model

base模型就是之前论文里的，譬如Youtube DNN。

![uD8sa0](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/uD8sa0.png)

#### DIN

可以和上面base model的框架做个对比，DIN多了一个`Activation Unit`，本质上是一个`Attention` 操作，根据`Candicdate Ad` 对用户历史行为做`加权和`。
$$
\boldsymbol{v}_{U}(A)=f\left(\boldsymbol{v}_{A}, \boldsymbol{e}_{1}, \boldsymbol{e}_{2}, \ldots, \boldsymbol{e}_{H}\right)=\sum_{j=1}^{H} a\left(\boldsymbol{e}_{j}, \boldsymbol{v}_{A}\right) \boldsymbol{e}_{j}=\sum_{j=1}^{H} \boldsymbol{w}_{j} \boldsymbol{e}_{j}
$$
![k2SAVE](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/k2SAVE.png)

与传统attention不同的是，这里稍微修改了每个item的权重限制，不局限于所有权重加和为1，目的是保留用户兴趣的强度。

#### 工程tricks

以上就是DIN的整体模型，接下去是在训练方面的一些小技巧。

-  **Mini-batch Aware Regularization**：针对CTR模型训练过程embedding计算量大的问题，通过考虑id出现频率，来自适应地调整正则化强度。对于出现频率高的，给与较小的正则化强度；对于出现频率低的，给予较大的正则化强度。

$$
L_{2}(\mathbf{W})=\|\mathbf{W}\|_{2}^{2}=\sum_{j=1}^{K}\left\|\boldsymbol{w}_{j}\right\|_{2}^{2}=\sum_{(\boldsymbol{x}, \boldsymbol{y}) \in \mathcal{S}} \sum_{j=1}^{K} \frac{I\left(\boldsymbol{x}_{j} \neq 0\right)}{n_{j}}\left\|\boldsymbol{w}_{j}\right\|_{2}^{2}
$$

- **Data Adaptive Activation Function**：

![ixWuUl](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/ixWuUl.png)

#### 参考

- [推荐系统中的注意力机制——阿里深度兴趣网络（DIN）](https://zhuanlan.zhihu.com/p/51623339)
- [推荐系统遇上深度学习(十八)-探秘阿里深度兴趣网络浅析及实现](https://zhuanlan.zhihu.com/p/38875771)

- [DIN算法代码详细解读](https://zhuanlan.zhihu.com/p/114244579)
- [CTR论文笔记:Deep Interest Network](https://zhuanlan.zhihu.com/p/263028916)