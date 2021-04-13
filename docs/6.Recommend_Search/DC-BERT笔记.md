来自SIGIR 2020

- [DC-BERT: DECOUPLING QUESTION AND DOCUMENT FOR EFFICIENT CONTEXTUAL ENCODING](https://arxiv.org/pdf/2002.12591.pdf)

提出一种弱交互的匹配召回模型，DC-BERT，主要包括三个组件

![QO2SBgF9jAkltcH](https://i.loli.net/2021/04/13/QO2SBgF9jAkltcH.png)

#### Dual-BERT component

底层双塔模型，检索速度较快。online bert负责query部分的encoding，offline-bert负责document部分的encoding（可以预先计算好缓存），实时计算复杂度为 $O(N_q L_q^2)$ ，其中$N_q$为query数量，$L_q$为query长度（Transfomer复杂度）

#### Transformer component

有论文指出BERT的较底层更关注`local syntax information`，而较上层更关注`complex semantics `，所以这里作者采用BERT的上面K层初始化交互Transformer。同时与BERT一样，引入pos embed 和 segment emb。

#### Classifier component

sigmoid交叉熵
$$
p\left(Q_{i}, D_{j}\right)=\sigma\left(\operatorname{MLP}\left(\left[o_{[C L S]} ; o_{[C L S]}^{\prime}\right]\right)\right)
$$

$$
\mathcal{J}=-\sum_{\left(Q_{i}, D_{j}\right)}(y \log (p)+(1-y) \log (1-p)),
$$

#### 实验

看一下速度对比

![gMxzUcO5XG8oPKl](https://i.loli.net/2021/04/13/gMxzUcO5XG8oPKl.png)

同时针对交互层做了ablation study，将Transformer替换为线性层和LSTM层做对比实验

![sOfVlIQhFMizoRy](https://i.loli.net/2021/04/13/sOfVlIQhFMizoRy.png)