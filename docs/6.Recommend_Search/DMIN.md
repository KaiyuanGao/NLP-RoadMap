来自阿里2020年的论文，背景同DIN一样，focus在推荐系统的ranking，CTR预估任务，

- Deep Multi-Interest Network for Click-through Rate Prediction

任务背景不多说了，参考DIN论文。

![KLVkVG](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/KLVkVG.png)

#### 输入

参考DIN，一样的。id特征过Embedding层

#### Behavior Refiner Layer

embedding层之后就是`Behavior Refiner Layer`，目的是对用户历史行为做更精细的表示学习。以往的模型都是直接拿embedding层之后的向量来用，这篇论文引入了`Multi-Head-Attention`进一步**学习序列信息及捕捉item间的关系**。
$$
\text { head }_{h}=\text { Attention }\left(x_{b} W_{h}^{Q}, x_{b} W_{h}^{K}, x_{b} W_{h}^{V}\right)=\operatorname{Softmax}\left(\frac{x_{b} W_{h}^{Q} \cdot\left(x_{b} W_{h}^{K}\right)^{T}}{\sqrt{d_{h}}} \cdot x_{b} W_{h}^{V}\right)
$$

$$
Z=M u l t i H e a d\left(x_{b}\right)=\operatorname{Concat}\left(h e a d_{1}, h e a d_{2}, \ldots, h e a d_{H_{R}}\right) W^{O}
$$

此外，还引入了一个辅助任务，通过用第$t + 1$ 个item的embedding表示监督$ z_t $ 的学习。


#### Multi-Interest Extractor Layer

学习到每个item的新表示之后，再往上就是对用户多兴趣的抽取。具体做法是，

1. 采用 `Multi-Head-Attention` 将每个item映射到 `h` 个表示，`h` 为用户兴趣数量；
2. 将每个head表示、目标item表示以及对应的位置向量，利用`Attention Unit` 处理，$a\left(\mathbf{I}_{j h}, \mathbf{x}_{t}, \mathbf{p}_{j}\right)$，其中 $I_{jh}$ 表示$j$个item的第`h`个head表示
3. 将所有item的相同序号head merge起来

$$
\text { interest }_{h}=\sum_{j=1}^{T} a\left(\mathbf{I}_{j h}, \mathbf{x}_{t}, \mathbf{p}_{j}\right) \mathbf{I}_{j h}=\sum_{j=1}^{T} w_{j} \mathbf{I}_{j h}
$$

