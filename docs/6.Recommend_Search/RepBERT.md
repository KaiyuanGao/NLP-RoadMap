来自论文：

- [RepBERT: Contextualized Text Embeddings for First-Stage Retrieval](https://arxiv.org/abs/2006.15498)

我们知道，在文本匹配或者文档检索任务中，对深度模型的探索主要有两种形式：**基于表示** 和 **基于交互**。如果区分更精细，也可以有**弱交互表示**，具体可以参考`ColBERT` 论文。



`RepBERT`  看名字就知道是一种基于表示的模型，具体模型设计也很简单，直接将query或者doc过BERT，是的，没看错，完全没有修改。两个BERT塔的参数共享，之后将最后一层出来的向量做average后当成是文本的表示，之后将query侧和doc侧的文本表示计算相似度打分。



训练过程，损失函数使用的是`MultiLabelMarginLoss`，通俗讲就是使正例和负例的距离尽可能远。
$$
\mathcal{L}\left(q, d_{1}^{+}, \ldots, d_{m}^{+}, d_{m+1}^{-}, \ldots, d_{n}^{-}\right)=\frac{1}{n} \cdot \sum_{1 \leq i \leq m, m<j \leq n} \max \left(0,1-\left(\operatorname{Rel}\left(q, d_{i}^{+}\right)-\operatorname{Rel}\left(q, d_{j}^{-}\right)\right)\right)
$$
负样本来源与一个batch内不同query的doc。

