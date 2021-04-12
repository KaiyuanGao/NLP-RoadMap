## 传统匹配模型

#### BM25

用于计算query和doc相似度得分的经典算法：
$$
\operatorname{Score}(Q, d)=\sum_{i}^{n} W_{i} R\left(q_{i}, d\right)
$$

其中Q为query，$q_i$ 为query中的单词，$d$ 为文档， $W_i$为单词权重
$$
W_{i}=\log \left(\frac{N-d f_{i}+0.5}{d f_{i}+0.5}\right)
$$
其中$N$为所有doc总数，$df_i$ 为包含$q_i$的doc数，整体类似idf的计算
$$
R\left(q_{i}, d\right)=\frac{f_{i}\left(k_{1}+1\right)}{f_{i}+K} * \frac{q f_{i}\left(k_{2}+1\right)}{q f_{i}+k_{2}}
$$
其中$f_i$为$q_i$在doc中出现的次数，$qf_i$为 $q_i$在query中出现的次数，$k_1, k_2, b$为超参，一般设置为2，1，0.75



## 深度匹配模型

#### DSSM

- [Learning Deep Structured Semantic Models for Web Search using Clickthrough Data](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/cikm2013_DSSM_fullversion.pdf)

- 数据来源点击日志，正例为query-doc有点，负例为随机采样的q-d无点
- 引入了`word hash`方法降低词表大小

![image-20210331190333838](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210331190333838.png)

#### CLSM (CNN-DSSM)

- [A Latent Semantic Model with Convolutional-Pooling Structure for Information Retrieval ](http://www.iro.umontreal.ca/~lisa/pointeurs/ir0895-he-2.pdf)
- 相比DSSM，优化以下几点
  - MLP替换成CNN+max-pooling
  - word trigram的基础上增加了letter trigram

![image-20210331191608850](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210331191608850.png)

#### LSTM-DSSM

- [SEMANTIC MODELLING WITH LONG-SHORT-TERM MEMORY FOR INFORMATION RETRIEVAL](https://arxiv.org/pdf/1412.6629.pdf)

- 编码器替换为LSTM

![image-20210331192148001](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210331192148001.png)

#### Convolutional Matching Model (ARC-I)

- [Convolutional Neural Network Architectures for Matching Natural Language Sentences](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1503.03244)
- 基于表示

![image-20210331192638208](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210331192638208.png)

#### Convolutional Matching Model (ARC-II)

- [Convolutional Neural Network Architectures for Matching Natural Language Sentences](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1503.03244)

- 基于交互

![image-20210331192807282](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210331192807282.png)

#### Convolutional Neural Tensor Network (CNTN)

- [Convolutional Neural Tensor Network Architecture for Community-based Question Answering](https://www.ijcai.org/Proceedings/15/Papers/188.pdf)
- 第一阶段将query和doc通过CNN+K-max-pooling表示成向量，第二阶段两者进行交互，第三阶段得出匹配得分

![image-20210331193626072](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210331193626072.png)

#### Neural Vector Space Model (NVSM)

- [Neural Vector Spaces for Unsupervised Information Retrieval](https://arxiv.org/pdf/1708.02702.pdf)
- 无监督

![image-20210331194442881](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210331194442881.png)

#### Deep CCA

![image-20210402154123385](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210402154123385.png)

#### MatchPyramid

- [Text Matching as Image Recognition](https://arxiv.org/abs/1602.06359)
- AAAI-2016
- 主要思想是将两段文本通过匹配矩阵(matching matrix) 类比成二维的图像，然后用CNN来处理；匹配矩阵中的每一个元素为词与词之间的相似度
- 关键在于如何构建`匹配矩阵`，尝试了三种方式：
  - 精确匹配：只有两个word完全相同才为1，否则为0
  - 余弦匹配：利用glove词向量，计算两个词的cos相似度
  - 点积匹配：利用glove词向量，计算两个词的点积

![image-20210402155513948](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210402155513948.png)

#### Match-SRNN

- [Match-SRNN: Modeling the Recursive Matching Structure with Spatial RNN](https://arxiv.org/abs/1604.04378)
- IJCAI2016
- 类似于一种动态规划的思路，query和doc的交互矩阵中每个位置的值由不仅由当前两个word的交互值得到，还由其前面（左、上、以及左上三个位置）的值决定
- neural tensor network 用于拟合query和doc中词的交互关系，Spatial RNN对于前一步得到的匹配矩阵进一步提取特征， 类似动态规划

![image-20210402170512914](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210402170512914.png)

#### Decomposable Attention Model

- [A Decomposable Attention Model for Natural Language Inference](https://arxiv.org/pdf/1606.01933.pdf)
- EMNLP 2016
- query和doc计算soft-align attention

![image-20210402171718631](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210402171718631.png)

![image-20210402172315759](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210402172315759.png)

#### MV-LSTM

- [A Deep Architecture for Semantic Matching with Multiple Positional Sentence Representations](https://arxiv.org/abs/1511.08277)
- AAAI-2016
- 首先利用Bi-LSTM分别对query和doc进行编码，token表示为前向和后向隐层的concat；接着对query和doc的每个词分别一一交互，得到匹配度矩阵，相似度计算有三种：consine、双线性、tensor layer；最后对多个匹配度矩阵计算k-max pooling 后接MLP得到匹配度打分



![image-20210405143143553](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210405143143553.png)

#### DRMM

- [A Deep Relevance Matching Model for Ad-hoc Retrieval](http://www.bigdatalab.ac.cn/~gjf/papers/2016/CIKM2016a_guo.pdf)
- CIKM'16
- 提出 `relavance matching` 和`semantic matching`两个虽然都是匹配任务，但还是有区别的。relavance matching需要额外重点考虑以下三点：
  - Exact matching signals：query中的term在doc中精确命中
  - Query term importance：query中term的重要性
  - Diverse matching requirement：多样的匹配需求？

- 模型方面，大框架和传统基于交互的模型大同小异，主要看以下`matching histogram mapping`。
  - 将query和document的term两两比对，计算相似性，再将相似性进行以直方图的形式进行分桶计算。例如：query: car ; document: (car, rent, truck, bump, injunction, runway)。query和doc两两计算相似度为（1，0.2，0.7，0.3，-0.1，0.1）。将[-1,1]的区间分为{[−1,−0.5], [−0.5,−0], [0, 0.5], [0.5, 1], [1, 1]} 5个区间。落在0-0.5区间（第三个区间）的个数有0.2，0.3，0.1共3个，所以最终表示为：[0,1,3,1,1]

![image-20210406113041856](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210406113041856.png)

#### aNMM

- [attention based Neural Matching Model](https://arxiv.org/abs/1801.01641)
- 

- 作者认为不同位置的word在卷积核中共享权重是不合理的【在NLP领域的文本匹配问题上，query和doc的匹配term可能出现在任意位置上，并不一定是强位置关联的】，因此提出了基于value的共享权重方法。权重共享的思路可以从下图的上半部分看出

![image-20210405150350142](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210405150350142.png)

#### ABCNN

- [ABCNN: Attention-Based Convolutional Neural Network for Modeling Sentence Pairs](https://arxiv.org/pdf/1512.05193v3.pdf)
- ACL2015
- 双塔模型BCNN：两个权重共享的CNN，

![image-20210405151756636](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210405151756636.png)

- 加入attention：ABCNN-1，整体和BCNN相似，不过在输入层引入了attention权重概念。具体而言，attention矩阵来源query和doc每个term的相近度：$1/(1+|x-y|)$，之后将attention matrix分别乘上一个参数W，得到attention feature，与原始的representation feature stack起来，作为模型输入。上层网络与BCNN相同

![image-20210405151924169](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210405151924169.png)

- 加入attention：ABCNN-2，相较于ABCNN-1，这里的attention层作用在卷积操作之后

![image-20210405152829840](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210405152829840.png)

- 加入attention：ABCNN-3，1和2的结合

![image-20210405153013803](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210405153013803.png)

#### MCAN

- [Multi-Cast Attention Networks for Retrieval-based Question Answering and Response Prediction](https://arxiv.org/abs/1806.00778)

- KDD2018

- 提出一种多视角的attention新框架。

  - 输入层通过highway encoder将原始的embedding经过编码输入下一层
  - attention层分为两部分co-attention以及multi-cast attention。对于co-attention层对于每个query-doc的pair对，使用max pooling、mean pooling以及alignment pooling三种attention机制，对query和doc本身则使用intra-attention(self-attention)
  - 通过前一层的co-attention，query和doc可以得到3种attention： alignment-pooling，max-pooling以及mean-pooling。对于query和doc则各自得到intra-pooling，一共有5个attention，每个attention可以得到原始向量与attention交互后的向量，通过concat、哈达码积和minus后，经过压缩函数进入下一层
  - 前面5种attention得到的网络经过LSTM编码，并经过mean-max池化后得到固定维度向量，经过两层的high way网络后套上softmax输出

  

![image-20210405155423295](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210405155423295.png)

#### BiMPM

- [Bilateral Multi-Perspective Matching for Natural Language Sentence](https://arxiv.org/abs/1702.03814)

- IJCAI2017

- 提出query->doc之外doc->query的多视角思路，

  ![image-20210405155833105](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210405155833105.png)

  - 双塔结构，输入词向量分别过双向LSTM
  - 重点在于匹配层，目的是分别从P-Q以及Q->P两个方向的相似度。给出了四种匹配策略，

  ![image-20210405160300547](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210405160300547.png)

#### ESIM

- [Enhanced LSTM for Natural Language Inference](https://arxiv.org/pdf/1609.06038.pdf)
- 目标是自然语言推断任务，即给定前提premise和假设hypothesis，要求判断两者的关系（1.不相干neural；2.冲突contradiction，即有矛盾，3.蕴含entailment，即能从p推断h或者两者表达相同的意思）
- 模型为`chain LSTM`，引入句子间注意力机制，来实现local inference，进而实现global inference

![gCXK3rWk8uPdmeS](https://i.loli.net/2021/04/12/gCXK3rWk8uPdmeS.png)

- Input Ecoding

$$
\begin{array}{l}
\bar{a}_{i}=B I L S T M(a, i), \forall i \in\left[1, \ldots, l_{a}\right] \\
\bar{b}_{j}=B i L S T M(b, j), \forall j \in\left[1, \ldots, l_{b}\right]
\end{array}
$$

- Local Inference Modelling

  - 计算相似度矩阵

  $$
  e_{i j}=\bar{a}_{i}^{T} \bar{b}_{j}
  $$

  - softmax加权

  $$
  \begin{array}{l}
  \tilde{a}_{i}=\sum_{j=1}^{l_{b}} \frac{\exp \left(e_{i j}\right)}{\sum_{k=1}^{l_{b}} \exp \left(e_{i k}\right)} \bar{b}_{j}, \forall i \in\left[1, \ldots, l_{a}\right] \\
  \tilde{b}_{j}=\sum_{j=1}^{l_{a}} \frac{\exp \left(e_{i j}\right)}{\sum_{k=1}^{l_{a}} \exp \left(e_{k j}\right)} \bar{a}_{i}, \forall j \in\left[1, \ldots, l_{b}\right]
  \end{array}
  $$

  - 拼接构造特征

  $$
  \begin{array}{l}
  m_{a}=[\bar{a} ; \tilde{a} ; \bar{a}-\tilde{a} ; \bar{a} \odot \tilde{a}] \\
  m_{b}=[\bar{b} ; \tilde{b} ; \bar{b}-\tilde{b} ; \bar{b} \odot \tilde{b}]
  \end{array}
  $$

- Inference Composition

  - 最后一层对两个部分过BI-LSTM之后接pooling接softmax，做pooling的原因是句子不定长

#### DIIN

- ICLR2018：[Natural Language Inference over Interaction Space](https://arxiv.org/abs/1709.04348)
- 背景也是NLI。编码层使用了glove初始化的word embedding，以及char embedding，以及语法特征（词性和精确匹配度）。交互层就是一个attention

![EUMuOQy4lrFXLpv](https://i.loli.net/2021/04/12/EUMuOQy4lrFXLpv.png)

#### MwAN

- IJCAI 2018：[Multiway Attention Networks for Modeling Sentence Pairs](https://www.ijcai.org/Proceedings/2018/0613.pdf)
- 背景是句子匹配任务。模型encoder使用的是双向GRU，匹配层利用多种attention计算之后融合，最上层进行softmax分类预测。
- 具体的，多种attention计算包括：拼接(concat),  双线性(bilinear), 点乘(dot)和 相减(minus)
- Inside Aggregation指的是不同种attention内部的聚合
- Mixed Aggregation指的是四中attention之间的聚合

![l8sAKRwbLPyHkzp](https://i.loli.net/2021/04/12/l8sAKRwbLPyHkzp.png)



## BERT相关

#### Sentence-BERT



#### BERT Doc Retrival

- [Simple Applications of BERT for Ad Hoc Document Retrieval](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1903.10972)
- 解决BERT对长文本doc匹配，提出一种加权的短句子得分方法来解决长文本匹配得分问题。具体而言，先对doc切句子，选取top-n，然后用短句子和query过BERT进行匹配得到打分，然后加权到doc级别的打分。注意，还需要考虑原始doc的BM25打分