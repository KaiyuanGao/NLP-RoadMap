来自阿里 AAAI 2019 的论文，还记得那篇经典的`DIN`嘛，这篇就是它的改进版本，`DIEN`

- Deep Interest Evolution Network for Click-Through Rate Prediction
- 代码：https://github.com/mouna99/dien

直接从论文题目也可以看到，仅仅是多了一个`evolution`，意思是进化，具体到推荐里面，就是说用户的兴趣是多样的（这点在DIN已经提及）且随时间进化/变化的（DIEN的重点）。

#### 和DIN的区别

深度兴趣网络（DIN）强调用户兴趣是多种多样的，它使用基于注意力的模型来捕获目标商品的相对兴趣，并获得自适应兴趣表示。 但是，包括DIN在内的大多数兴趣模型都直接将行为视为兴趣，而潜在的兴趣很难通过显式行为充分反映出来。 先前的方法忽略了在行为背后挖掘用户的真正兴趣。 此外，用户兴趣不断发展，获取兴趣的动态对于兴趣表示很重要。

简而言之，DIN没有考虑用户历史之间的时序关系, DIEN则使用了GRU来建模用户历史的时间序列。

 

#### 模型

![PqMZQ9](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/PqMZQ9.png)

任务背景、输入特征啥的都跟DIN的一样，来看看不一样的几个模块

#### Interest Extractor Layer

兴趣抽取模块，采用的是GRU来建模用户历史行为，
$$
\begin{array}{l}
\mathbf{u}_{t}=\sigma\left(W^{u} \mathbf{i}_{t}+U^{u} \mathbf{h}_{t-1}+\mathbf{b}^{u}\right) \\
\mathbf{r}_{t}=\sigma\left(W^{r} \mathbf{i}_{t}+U^{r} \mathbf{h}_{t-1}+\mathbf{b}^{r}\right) \\
\tilde{\mathbf{h}}_{t}=\tanh \left(W^{h} \mathbf{i}_{t}+\mathbf{r}_{t} \circ U^{h} \mathbf{h}_{t-1}+\mathbf{b}^{h}\right) \\
\mathbf{h}_{t}=\left(\mathbf{1}-\mathbf{u}_{t}\right) \circ \mathbf{h}_{t-1}+\mathbf{u}_{t} \circ \tilde{\mathbf{h}}_{t}
\end{array}
$$
此外，由于真实标签仅仅是通过最后一个item触发的，所以无法有效地监督学习历史的状态。为此，提出了一个辅助任务，用t时刻的item去监督学习（t-1）时刻的隐状态表示。具体做法是对比学习的思路，N个负样本和一个正样本，损失函数如下，
$$
\begin{aligned}
L_{a u x}=-& \frac{1}{N}\left(\sum_{i=1}^{N} \sum_{t} \log \sigma\left(\mathbf{h}_{t}^{i}, \mathbf{e}_{b}^{i}[t+1]\right)\right.\\
&\left.+\log \left(1-\sigma\left(\mathbf{h}_{t}^{i}, \hat{\mathbf{e}}_{b}^{i}[t+1]\right)\right)\right)
\end{aligned}
$$
最终CTR任务的建模目标为：
$$
L=L_{\text {target }}+\alpha * L_{a u x}
$$


#### Interest Evolving Layer

兴趣演化层，采用的是attention机制，输入是GRU的隐藏表示和target item的向量表示。
$$
a_{t}=\frac{\exp \left(\mathbf{h}_{t} W \mathbf{e}_{a}\right)}{\sum_{j=1}^{T} \exp \left(\mathbf{h}_{j} W \mathbf{e}_{a}\right)}
$$
对上述attention score有多种应用方式：

- GRU with attentional input (AIGRU)
- Attention based GRU(AGRU) 
- GRU with attentional update gate (AUGRU)

#### 参考

- [详解阿里之Deep Interest Evolution Network(AAAI 2019)](https://zhuanlan.zhihu.com/p/50758485)
- [也评Deep Interest Evolution Network](https://zhuanlan.zhihu.com/p/54838663)