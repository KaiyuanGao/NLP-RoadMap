# ERNIE-Enhanced Retrieval for Large-Scale Web Search

来自百度的论文：

- Pre-trained Language Model for Web-scale Retrieval in Baidu Search

In this paper, we introduce a novel retrieval sys- tem that is fully deployed in Baidu Search,  `the dominant search engine in China`

应该是BS语义索引召回的ERNIE模型，面向对象是整个库内网页。

### 模型

任务目标是建模 query-doc之间的相关性匹配（与 语义匹配有区别）。模型骨架是ERNIE，参考了poly-encoder的顶层交互方式，如下图：

![VWm8I29K5YXehAf](https://i.loli.net/2021/04/17/VWm8I29K5YXehAf.png)

- 底层quey、doc端ERNIE参数共享
- 顶层query端：用16个参数向量分别对Encoder的输出序列表示做scaled_dot_product_attention，并经过FC转换后产出16个256维的向量表示
- 顶层title端：直接取Encoder最后的CLS向量，经过FC转换后产出1个256维的向量表示
- 训练：query端的16个向量分别与title端的向量做内积，然后对16个内积取max产出最终的score
- 线上inference：直接将query端16个向量做avg-pooling，这样只需和title-emb做一次内积

整个训练过程分四步：

1. ERNIE 通用预训练
2. ERNIE 领域预训练（query + ttile，单塔）
3. ERNIE 一次任务微调（query/title，双塔，搜索日志）
4. ERNIE 二次任务微调（query/title，双塔，人工标注数据）

![7bMZpPrK41oaCVf](https://i.loli.net/2021/04/18/7bMZpPrK41oaCVf.png)

##### 损失优化

$$
\theta^{*}, \beta^{*}=\arg \min _{\theta, \beta}-\sum_{q} \sum_{d^{+} \in \tilde{D}_{q}^{+}} p\left(d^{+} \mid q, d^{+}, \hat{D}_{q}^{-}\right) .
$$

$$
p\left(d^{+} \mid q, d^{+}, D_{q}^{-}\right)=\frac{\exp \left(f\left(q, d^{+}\right) / \tau\right)}{\exp \left(f\left(q, d^{+}\right) / \tau\right)+\sum_{d^{-} \in D_{q}^{-}} \exp \left(f\left(q, d^{-}\right) / \tau\right)}
$$

### 数据

数据来源主要是：搜索日志（大规模自动样本、有点无点）、人工标注（0-4分档）

上述样本作为`hard negative`，由于召回是从大量不相关doc中找到非常小部分的相关doc（$\left.\left|D_{q}^{+}\right| \ll\left|D_{q}^{-}\right| \approx|D|\right)$），所以还设计了`easy negative`，即在同一batch中随机sample。

![Kh6sNfBAy82Za5z](https://i.loli.net/2021/04/18/Kh6sNfBAy82Za5z.png)

### 部署

##### 向量压缩和量化

简单说，压缩就是把768的ERNIE向量通过FC转成256维度，量化就是把float32的变成int8。

##### 召回框架

![fZscLpwq4XrvjyB](https://i.loli.net/2021/04/18/fZscLpwq4XrvjyB.png)

- 离线部分：计算doc端的embedding，cache保存
- 在线部分：分为召回和排序。召回部分又包括传统query特征计算，以及ERNIE-query向量计算。ERNIE-query-emb和ERNIE-doc-emb计算相似度打分，与传统特征一起作为上层排序的特征，排序模型为RankSVM，进一步缩小召回候选集

### 实验

