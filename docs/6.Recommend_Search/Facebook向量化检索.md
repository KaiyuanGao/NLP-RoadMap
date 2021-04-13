论文主题是介绍Facebook的语义检索系统，包括数据、模型、部署以及全栈优化几个方面，非常细节和工程trick。

- 论文：[Embedding-based Retrieval in Facebook Search](https://arxiv.org/abs/2006.11632)
- 源码：
- 会议：KDD 2020

#### 问题 & 目的

- 搜索引擎的召回问题，针对传统的关键term匹配方法存在的问题，引入向量语义匹配召回，embedding-based retrieval （但通常两者需要结合使用）
- 针对facebook场景，社交网络上的检索问题不仅仅关注于query的文字信息，还要关注于用户的上下文信息
- 整体框架，在每条请求到来的时候会实时计算用户的 embedding，然后利用构建好的 document embedding 倒排索引做 retrival；为了加速，在向量化召回中还会采用 Quantization 技术

![image-20210302201248286](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210302201248286.png)

#### 模型

- 双塔模型：query-encoder 和 document-encoder，输入的特征除了文本之外还可以包含与用户相关的信息（比如location feature 和 social embedding feature）
- 相似度函数：cosine相似度
- 损失函数：triple loss

$$
L=\sum_{i=1}^{N} \max \left(0, D\left(q^{(i)}, d_{+}^{(i)}\right)-D\left(q^{(i)}, d_{-}^{(i)}\right)+m\right)
$$







![image-20210302201746343](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210302201746343.png)

#### 训练样本挖掘

样本的选择比较有意思，

- 负样本的选择：以有点数据作为正样本，有如下两种负样本方案：
  - 随机负样本
  - 有展现但无点击负样本
  - 结论是：有展现无点击负样本效果很差，recall低了55%
  - 原因是：训练数据和实际线上数据的分布偏差。`We believe it is because these negatives bias towards hard cases which might match the query in one or multiple factors, while the majority of documents in index are easy cases which do not match the query at all`
- 正样本的选择：
  - 有点击
  - 有展现
  - 结论是：两者效果相近，即使是在展现的正例基础上叠加点击正例，结果也没有进一步的提升
  - 引申：基于上述结论，个人认为展现作为正例可能更合适。原因是，点击样本数有限，且未点击的样本并不一定就是差的，可以提升多样性等等。
- 难样本挖掘：前面`有展现无点击`负样本就是hard case，思路是只用easy case学的模型效果也不一定好。
  - hard nagative mining
    - Online hard negative mining：训练时每个batch中的正例随机分配给不通query组成负例
    - Offline hard negative mining：上述还不够hard，毕竟是不同query的doc。在每个 query 的所有 document 中，选择那些排序在 101-500 的位置的样本作为 hard nagative。
    - 结论是：效果提升
  - hard positive mining
    - 从失败的 search session 日志中找到那些没被系统召回的但是 positive 的样本



#### 部署

采用的是ANN，且通过 quantization 来进一步缩短向量间相似性的计算时间，是一种向量压缩的技术，参考[理解 product quantization 算法](http://vividfree.github.io/机器学习/2017/08/05/understanding-product-quantization)。Fasis库实现，

- coarse quantization：通过聚类的方法将整个候选库中的分为若干个 clsuter，在query 到来的时候，选出 topk 个 cluster
- product quantization：计算分数距离进行排序

#### 召回与精排优化

> since the current ranking stages are designed for existing retrieval scenarios, this could result in new results returned from embedding based retrieval to be ranked sub-optimally by the existing rankers

即新的语义召回结果可能不被精排系统认可，解决办法

- 将召回的 embedding 作为精排模型的特征
- 通过人工的方式对被召回的结果重新打上 label，重训模型

#### 其他

- [知乎-基于Bert的语义检索框架](Beyond Lexical: A Semantic Retrieval Framework for Textual Search)
- 京东：towards personalized and semantic retrieval : an end to end solition for e-commerce search via embedding learning