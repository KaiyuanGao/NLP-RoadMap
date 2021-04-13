## Understanding the Behaviors of BERT in Ranking

清华大学利用BERT做的一系列排序任务实验，主要基于两个数据集：

- MS MARCO：QA问答数据集，主要是从Bing上的用户搜索日志中的一些query，以及对应的一些候选passage，该任务的要求便是从候选的passage中选择能够回答该query的正确passage，包含一百多万个query和一百多万个passage
- ClueWeb：通过query检索相应的document

### 模型设计

一共设计了四组实验

- BERT(Rep)：query和doc分别过BERT，得到向量后，计算cos相似度
- BERT(Last-Int)：interaction-based ranker，将query和doc通过sep拼接后，过BERT，拿到cls向量，过FC
- BERT(Mult-Int)：同上，不同的是拿每一层的CLS向量进行加权求和
- BERT(Term-Trans)：将query中的各个词与doc中的所有词计算cos相似度，然后求平均，再将各个层的这些token级别的匹配分数进行加权求和

另外作为对比，使用了四组基线模型：

- base版模型: MS MARCO上用的是BM25，ClueWeb上使用Galago SDM
- Learning2Rank: MS MARCO上用的是RankSVM，ClueWeb上使用Coordinate Ascent
- K-NRM: 核心思想是kernel pooling的一个query-document匹配模型
- Conv-KNRM: K-NRM的n-gram版本

### 实验结论

![image-20210405113411915](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210405113411915.png)

上图左半部分，MS MARCO数据集：

- BERT(Rep)效果几乎是瞎猜，而BERT(Last-Int)效果最好，是否在这种排序问题中，BERT当成representation不合适，需要更多的交互才行？

上图右半部分，ClueWeb数据集：

- BERT系列模型几乎效果都作负，而在Bing click数据上进行了pretrain的Conv-KNRM模型效果是最好的。说明在ranking任务中，上下文语义并不是很重要，起码要比点击类型的信号要差一些，需要更多维度的信号

![image-20210405114440670](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210405114440670.png)

Attention可视化：标记类型的词（[SEP]、[CLS]）很重要