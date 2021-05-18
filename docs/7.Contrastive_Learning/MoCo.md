来自 FAIR, CVPR2020 的论文

- Momentum Contrast for Unsupervised Visual Representation Learning
- 代码：https://github.com/facebookresearch/moco

论文主要关注在无监督视觉表示。相比无监督表示学习在NLP领域的流行，目前CV领域主要还是以有监督预训练为主，造成这一差距的原因在于，它们对应的信号空间不同。语言任务有着离散的信号空间（词等），可以用于构建成分词后的词典（dictionary）。这种词典是无监督学习可以依赖的特征。但是，计算机视觉与之相反，其原始信号是在一个连续且高维的空间中，无法成为结构化信号用于人类的交流。

> 对比学习可以认为是训练一个编码器用于查字典的任务

![MhtSZX](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/MhtSZX.png)

为了增加训练时负样本的数量（负样本数量直接影响对比学习的性能，有研究表明本质上是hard negative sample引起的），**MoCo**提出使用动量编码器来编码负样本：使用一个队列用来缓存其他batch的图像表示，作为负样本动量编码器的参数是主编码器参数的滑动平均（moving average），因此其编码的负样本能同时保证时效性。



文章目标函数采用的是 `InfoNCE`
$$
L_{q}=-\log \frac{\exp \left(q * k_{+} / \tau\right)}{\sum_{i=0}^{K} \exp \left(q * k_{i} / \tau\right)}
$$
