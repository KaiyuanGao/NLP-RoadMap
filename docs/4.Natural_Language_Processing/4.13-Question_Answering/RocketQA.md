来自百度的论文，主要是提出了一种针对开放域问答、检索领域的训练方法，

- [RocketQA: An Optimized Training Approach to Dense Passage Retrieval for Open-Domain Question Answering](https://arxiv.org/abs/2010.08191)

首先来看看检索场景中存在的问题：

- 训练阶段与预测阶段的gap
  - 训练过程由于受计算资源的限制，模型见到的负样本有限，可以说远远小于预测阶段所需要区分的负样本
- 训练数据中可能存在大量未标注的正例
  - 即，一个段落中可能存在多个正确答案，但是标注只标出了一个，导致在负采样时容易采样到未标注出来的正例
- 人工标注的数据集规模小，成本大

针对以上问题，RocketQA提出三种训练技巧：

- cross-batch random negative
- denoised hard negative sampling
- data augmentation

模型比较简单，就是双塔ERNIE，取[CLS]作为query/doc的表示，在上层直接做点乘算距离，然后loss的话就是使得正例在所有候选项里打分最高
$$
\begin{aligned}
& \mathcal{L}\left(q_{i},\left\{p_{i, j}^{-}\right\}_{j=1}^{m}\right) \\
=&-\log \frac{e^{\operatorname{sim}\left(q_{i}, p_{i}^{+}\right)}}{e^{\operatorname{sim}\left(q_{i}, p_{i}^{+}\right)}+\sum_{j=1}^{m} e^{\operatorname{sim}\left(q_{i}, p_{i, j}^{-}\right)}}
\end{aligned}
$$

#### Cross-batch Negatives

在训练双塔模型时，最常用的就是`in-batch negatives`，为了更充分采样负样本，提出了`cross-batch negatives`。

主要思想就是在多GPU训练时，在GPU间共享doc向量，使得一个正例见到的负样本从原本的 $N-1$ 变成了 $M*N-1$ 。$N$ 是一个batch内query数量， $M$ 是GPU数量。

![67hrF8AJNKZBQ2m](https://i.loli.net/2021/04/21/67hrF8AJNKZBQ2m.png)

#### Denoised Negative Sampling

另外一个重要问题是，如何增加 `hard negatives`。很多工作都表明`hard negatives`对最终效果影响较大，以及探索如何采样`hard negatives`，比如Facebook的那篇。

一种常见的做法是，选取除正确答案之外的排序靠前的那些候选项，但是这种做法在论文数据集下（MSMARCO），会引入 假负例`false negatives`。那么，顺着这个思路，自然就会考虑到从排序靠前的非正确候选集合中，清洗掉那些假负例。具体说，可以使用一个`cross-encoder`结构模型来完成，毕竟底层交互效果更好。把那些候选集里打分超过某一阈值的doc，我们认为这些就是假负例，把剩余的用作`hard negatives`。

#### Data Augmentation

常规的迁移学习增强样本操作，利用原始数据集训练一个`cross-encoder`模型，然后对一批新query进行预测，但是模型肯定会存在误判问题，为了尽可能避免，就取置信度高的那些样本即可。

#### 模型训练

一开始看论文题目还在想，为啥叫做 `RocketQA`，拍脑门也得有个原因吧，果然在模型训练部分看到了解释。作者把`multi-stage`的训练看成是火箭`Rocket` 发送过程，来看具体训练步骤：

- step-1：使用有标注的训练数据训练一个双塔`dual-encoder` $M_{D}^0$
- step-2：使用有标注的训练数据训练一个交互式单塔`cross-encoder` $M_C$。其中负样本来自 $M_{D}^0$ 的排序靠前的非正确候选项
- step-3：上文介绍过的`Denoised Negative Sampling`，利用$M_C$构建`hard negatives`，训练双塔`dual-encoder` $M_{D}^1$
- step-4：利用$M_C$ 数据增强，继续训练得到双塔`dual-encoder` $M_{D}^2$

![i13ocVR6dkxe7K9](https://i.loli.net/2021/04/22/i13ocVR6dkxe7K9.png)

#### 实验结论

数据集使用了 微软 MSMARCO 和谷歌 Natural Question 数据，

![BLoh5R1fFeiIKm6](https://i.loli.net/2021/04/22/BLoh5R1fFeiIKm6.png)

#### 消融研究

主要是看看论文里的三个新策略会带来什么样的影响。

![qwzxQvJrmP1ikYh](https://i.loli.net/2021/04/22/qwzxQvJrmP1ikYh.png)

-  `cross-batch` 的效果优于`In-batch`，表明负样本增多减小了训练和预测阶段的gap；进一步分析了具体负样本数量对效果的影响 

![FBRSk6Tngv7AiE1](https://i.loli.net/2021/04/22/FBRSk6Tngv7AiE1.png)

- 未去噪的强负样本对效果产生副作用，去噪后效果增益明显
- 数据增强也有较大增益