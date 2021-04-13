来自论文：Cross-Thought for Sentence Encoder Pre-training



### 背景

一种高质量的句子表示，句子表示有很多应用，比如召回中query或doc的表示。目前已有的句子表示基本为：

- BERT的CLS向量（只针对NSP进行优化，表示信息有限）
- BERT的最后一层取平均（会把句子中无用的信息考虑进来，增加噪音）

文中做法是：**用周边其他句子的表示预测当前句子的mask token**。

### 模型

输入数据为文本切分的短句，不进行shuffle，同时每一个句子前拼上N个special token（5个最好），类似cls，用来表义句子向量。



预训练：

- 每个训练句子过12层的Transformer，取出前面的special token（大红点）作为句子表示；
- 将大红点作为一个序列过cross-sequence Transformer，用于句子之间的信息交互，得到一个新的句子表示；同时会得到句子之间的attention权重，可应用与下游任务
- 最后把新的句子表示拼接上第一步的token表示，过一层Transformer，预测句子中被mask掉的词



微调：尝试了两个任务：Answer Selection 和 sequence-pair classification

- Answer Selection：q过12层Transformer，和预先计算好的answers过cross-sequence transformers，得到各个answer的attention weight，计算ranking-loss

$$
\mathcal{L}_{\text {answer }}=-\log (\alpha[0][m])
$$

- sequence-pair classification：分别对两个句子进行编码，取出句子表示，输入cross-sequence transformers得到融合后的句子表示；然后拼接起来做softmax



![image-20210407205809228](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210407205809228.png)

### Case Study

![image-20210407212113811](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210407212113811.png)