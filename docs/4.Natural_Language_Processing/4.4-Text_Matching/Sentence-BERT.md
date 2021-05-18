- 论文：[Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks](https://arxiv.org/abs/1908.10084)

- 代码：https://github.com/UKPLab/sentence-transformers

整体思路比较简单清晰，使用场景为文本相似度匹配，如果直接使用BERT的话，这种`cross-encoder`的结构复杂度非常高。举个栗子，有10000个句子，我们想要找出最相似的句子对，需要计算（10000*9999/2）次，论文中指出，在V100的GPU上，需要大约65个小时。

一种常见的方法是，直接利用BERT产出句子的向量（利用CLS或者对输出层向量取平均），然后预先存储下来，线上直接计算cos相似度即可。但这有一个问题，有实验指出，这种方式得到的句子/文档向量，甚至比Glove向量平均还要差！

#### SBERT模型

![dj3x8twHIYeE2nu](https://i.loli.net/2021/04/22/dj3x8twHIYeE2nu.png)

这种双塔式结构，可以将上述栗子中的 `65h` 优化到 `5s` ！