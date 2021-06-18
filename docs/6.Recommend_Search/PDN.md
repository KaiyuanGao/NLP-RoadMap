

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

图中虚线为用户user和target item直接路径，实线是用户user和target item经过交互item的两跳路径。

![](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/HotGmT.png)

#### PDN模型

整体框架如下，包括几个模块：Embedding layer、Trigger Net、Similarity Net、DirectNet、BiasNet。

![7yUp9U](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/7yUp9U.png)

先来全局角度看看模型是怎么运行的。
$$
\hat{y}_{u i}=A G G\left(f_{d}\left(z_{u}, x_{i}\right),\left\{P A T H_{u j i}\right\}\right) \text { with } j \in N(u)
$$

$$
P A T H_{u j i}=M E G\left(\operatorname{TrigNet}\left(z_{u}, a_{u j}, x_{j}\right), \operatorname{SimNet}\left(x_{j}, c_{j i}, x_{i}\right)\right)
$$

其中，

- $f_d$ 表示直接路径权重的计算函数，即DirecNet部分；
- $PATH_{uij}$ 表示基于交互item $j$  的二跳路径权重；
- AGG 表示融合聚合函数，得到最终相关性打分；
- MEG 表示计算每条二跳路径权重的函数

下面来具体看每个小模块

#### 输入特征和Embedding

输入特征包括四个域

- 用户域  $z_u$
- 用户行为域 $a_{uj}$
- 商品共现域 $c_{ji}$
- 商品域 $x_i$

特征会包括离散和连续的，都转成one-hot表示后，embedding到固定的size。

#### Trigger Net 

trigger net用于计算二跳路径中的第一跳，目的是计算用户的多兴趣点，这里trigger可以理解为兴趣。具体计算为，将用户向量、交互商品向量、两者对应的用户行为向量concat起来过MLP，得到的为用户对该商品的偏好度。
$$
t_{u j}=\operatorname{TrigNet}\left(z_{u}, \boldsymbol{a}_{u j}, x_{j}\right)=\operatorname{MLP}\left(\operatorname{CAT}\left(E\left(z_{u}\right), E\left(\boldsymbol{a}_{u j}\right), E\left(x_{j}\right)\right)\right)
$$
注意，这里对不同用户出来的维度是不同的，维度大小等于用户的trigger。

#### Similarity Net

sim net用于计算二跳路径中的第二跳，目的是计算交互商品与目标商品的相关度。具体计算为，将交互商品向量、目标商品向量、两者共现信息向量concat起来过MLP，
$$
s_{j i}=\operatorname{SimNet}\left(x_{j}, c_{j i}, x_{i}\right)=\operatorname{MLP}\left(\operatorname{CAT}\left(E\left(x_{j}\right), E\left(c_{j i}\right), E\left(x_{i}\right)\right)\right)
$$
注意，这里也是对不同用户出来的维度是不同的，维度大小等于用户的trigger。

二跳路径最终的计算权重：
$$
P A T H_{u j i}=\operatorname{MEG}\left(t_{u j}, s_{j i}\right)=\ln \left(1+e^{t_{u j}} e^{s_{j i}}\right)
$$

#### DirectNet

用于计算直接路劲，目的是直接建模U和I的关系。具体就是，双塔建模顶层交互（点积）
$$
d_{u, i}=\boldsymbol{p}_{u} \boldsymbol{q}_{i}^{T}=\operatorname{MLP}\left(E\left(z_{u}\right)\right) \operatorname{MLP}\left(E\left(x_{i}\right)\right)^{T}
$$

#### Bias  Net

推荐系统的bias是一个很重要的分支。该模块目的就是de-bias，在训练时使用，serving时丢弃。

#### 损失函数

建模用户点击目标商品的概率
$$
\begin{array}{l}
\hat{y}_{u, i}=\text { softplus }\left(d_{u, i}\right)+\sum_{j=1}^{n} P A T H_{u j i}+\text { softplus }\left(y_{\text {bias }}\right) \\
p_{u, i}=1-\exp \left(-\hat{y}_{u, i}\right)
\end{array}
$$

#### 参考

- [SIGIR2021 | 超越I2I和向量内积，淘宝新一代召回范式：PDN模型](https://mp.weixin.qq.com/s/NznGxYghZZI-h_gnE2cu3A)