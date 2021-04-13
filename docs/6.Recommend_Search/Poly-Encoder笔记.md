来自FAIR的ICLR2020论文，Poly-Encoder是一种介于Bi-Encoder和Cross-Encoder之间的模型，结合了两者的优势。

- [Poly-encoders: Transformer Architectures and Pre-training Strategies for Fast and Accurate Multi-sentence Scoring](https://arxiv.org/pdf/1905.01969v2.pdf)

#### 模型

和ColBERT类似，属于later-interaction的匹配模型。

![rVMqgl7615xpHTu](https://i.loli.net/2021/04/12/rVMqgl7615xpHTu.png)

文中讨论的是问答，对应到query-doc retrieval也是一样的。左边塔是query，可以实时在线编码；右边塔是doc，可以提前缓存计算。具体而言

- doc侧，transformer编码，得到一个向量表示
- query侧，transformer编码，得到N个向量，从中选取m个向量表示（由于N在论文中一般很长，其实如果是query的话，可以考虑直接使用N个向量）
- query侧产出的m个向量与doc侧的一个向量做attention

query侧m个向量是怎么选出来的呢？作者尝试了两种方式，经过实验论证，发现简单暴力地直接选取前m个向量效果最佳。。。至于为啥作者也没详细解释。。。

实验效果这边就不展示了，毕竟效果不好的话也发不出文章（狗头）实际地推断速度倒是可以看看对比效果：

![k5g14dPsLw7xuXF](https://i.loli.net/2021/04/12/k5g14dPsLw7xuXF.png)

#### 思考

