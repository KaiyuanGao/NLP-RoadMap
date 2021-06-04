国外网站 pinterest 关于用户多兴趣推荐的论文，发表于 KDD 2020，

- PinnerSage: Multi-Modal User Embedding Framework for Recommendations at Pinterest

目前工业中大家用的比较多的都是单一的用户向量，但是这种形式不能很好很全面地表示用户，因此需要`multi-embedding`。目前存在的难点是：

1. 对于特定的用户，多少个embedding是比较合适的呢？
2. multi-embedding相对于single-embedding，面对大规模数据，如何线上inference与update？
3. 在多个embedding中，如何选择个性化？



回顾下传统iterm-item向量召回，一般有两种做法：

1. 选择用户短期历史行为的每个视频，进行多次ANN查找选出近邻视频，这样的做法不仅时间成本高而且推出视频同质化严重；
2. 将用户短期历史行为的所有视频embedding进行pooling形成代表用户的user embedding，再进行ANN近邻查找，这样的方式能一定程度的融合信息减少时间空间代价，但很容易造成信息损失， pooling出的embedding如图3所示很可能差了十万八千里。

![mzlGdO](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/mzlGdO.png)

PinnerSage则取两者之长，**对用户历史行为中的item进行聚类分组，pooling形成多个兴趣向量**。聚类既避免了多次ANN带来的压力，也能一定程度上避免信息损失。

前提：PinnerSage推荐系统中的item-emb是由Pinterest团队之前发布的PinSage图网络模型学习得到。

PinnerSage 聚类多兴趣召回分为两步走：

1. **聚类（Ward算法）**：对用户交互过的所有item进行聚类操作，Pinnersage聚类采用了hierarchical clustering聚类方法，并不需要像K-Means设置初始类别数，而是首先将每一个视频均看作一类，接下来每两类开始合并，若合并后组内variance增加最少则可以将两类合并为一类，直到variance超过阈值即停止。
2. **获取每个cluster的embedding**： 不是对每个簇内向量取平均，而是选择一个簇内的真实向量，使其与簇内其他向量距离之和最小。

$$
\text { embedding }(C) \leftarrow P_{m}, \text { where } m=\arg \min _{m \in C} \sum_{j \in C}\left\|P_{m}-P_{j}\right\|_{2}^{2}
$$

**聚合之后的类还是太多怎么办？** 聚后的类太多，还是会增加耗时和对Faiss的压力。PinnerSAGE的作法是**给每个类都打一个importance score，然后按这个分数做重要性采样**，文中说每次只采样3个cluster就足够满足用户多样性的需求。

**聚类多兴趣召回通过简单的策略便形成了用户多个兴趣，时间代价较少。但由于依赖其他算法形成的embedding空间，学习到的多个兴趣embedding很容易有偏，推出内容趋于高热难以满足个性化。**