做搜索相关性项目的时候，模型效果不好，于是去分析了ERNIE模型里attention头的可视化，发现有些头的注意力是对的，而有些根本就没有学到注意力，这些没学习好的head在最终聚合的时候会对效果作负。就想能着不能把这些部分去掉，对效果会不会有正向作用。一搜谷歌，果然有论文......

是ACL2019的论文

- [Analyzing Multi-Head Self-Attention: Specialized Heads Do the Heavy Lifting, the Rest Can Be Pruned](https://arxiv.org/abs/1905.09418)
- [开源代码](https://github.com/lena-voita/the-story-of-heads)

论文主要关注的是机器翻译任务，主要回答了以下几个问题

- 翻译质量在多大程度上取决于各个编码器头？
- 各个编码器头是否发挥一致和可解释的作用？ 如果是这样，那么对于翻译质量而言，哪些是最重要的？
- 哪种类型的注意力（encoder self-attention,、decoder self-attention、 decoder-encoder attention）对注意力头的数量以及层数（位置）最敏感？
- 是否可以在保证翻译效果的同时减少注意力头的数量？如何做？

#### 实验

使用Layer-wise relevance propagation (LRP) 以及 confidence（自信度）来衡量一个head的重要性。

![8XqEwdDNLvYPhV6](https://i.loli.net/2021/04/17/8XqEwdDNLvYPhV6.png)

#### 

分析不同head具体关注的方面

![D3daXyxq5Czvsbf](https://i.loli.net/2021/04/17/D3daXyxq5Czvsbf.png)



尝试对不同head进行剪，分析是否有冗余性

![p1CbXsanlfz8jAO](https://i.loli.net/2021/04/17/p1CbXsanlfz8jAO.png)

#### 结论

1. 在所有模型的第一层，总有一个头去关注rare words
2. head数量并不是越多越好，去掉一些头效果依旧有不错的效果(效果些许下降，也许是由于参数量下降引起的) 。作者理解是，当有足够多的head，已经能够关注位置信息，语法信息，关注罕见词的能力了，再多一些头，可能是enhance也可能是noise。