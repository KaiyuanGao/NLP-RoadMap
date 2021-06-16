来自阿里的论文，主要focus在推荐系统召回阶段的**多兴趣召回**，

- Multi-Interest Network with Dynamic Routing for Recommendation at Tmall

![天猫个性化首页](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/mMOoRr.png)

如上图左边是Tmall App首页，会针对不同用户展现不同的页面；右边是不同用户的交互历史，可以发现不同用户的兴趣不同，相同用户不同时间的兴趣也会有所不同。因此像Youtube那类的模型是无法达到千人千面的效果，使用一个User Embedding不能满足，MIND论文的改进就是针对一个用户会有多兴趣的Embedding表示。

## 问题定义

工业推荐系统中召回可以表示为如下三元组，$<I_u; P_u; F_i>$ ，$I_u$ 表示用户历史交互过的商品，$P_u$ 表示用户历史基本特征（年龄性别等）， $F_i$ 表示目标商品。于是MIND的任务流程为：

1. 找到合适的映射函数 ![[公式]](https://www.zhihu.com/equation?tex=f_%7Buser%7D) 来表征![[公式]](https://www.zhihu.com/equation?tex=V_u+%3D+f_%7Buser%7D%28I_u%2CP_u%29) ，其中 ![[公式]](https://www.zhihu.com/equation?tex=V_u+%3D+%5Cvec%7Bv_u%5E1%7D%2C%5Cvec%7Bv_u%5E2%7D%2C%2C%2C%5Cvec%7Bv_u%5EK%7D) 是一个维度为K*d的向量，**一个用户对应多个用户向量**，K表示用户向量的个数，d是每个用户向量的维度
2. 根据target item的embedding映射函数得到 ![[公式]](https://www.zhihu.com/equation?tex=%5Cvec%7Be_i%7D+%3D+f_%7Bitem%7D%28F_i%29)
3. 那么user和target item的距离为： ![[公式]](https://www.zhihu.com/equation?tex=f_%7Bscore%7D%28V_u%2C%5Cvec%7Be_i%7D%29%3D%5Cmax++%5Climits_%7B1%5Cleq+k+%5Cleq+K%7D+%5Cvec%7Be_i%5E%7BT%7D%7D+%5Cvec%7Bv_u%5Ek%7D)
4. 根据user和每个target item的得分，取topN得到用户距离最近的topN个item

## 模型架构

![cG6ADG](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/cG6ADG.png)

#### 输入

MIND的输入包括三个部分，

1. 最左侧是用户向量，包括性别、年龄等用户画像
2. 中间的是用户行为特征，即产生行为（历史点击或购买等）的Item列表和Item属性列表，item向量集合；
3. 最右侧是标签label，即用户下一次点击或购买的item；

输入进行Embedding操作之后，转成稠密向量，然后将用户行为序列的emb表达进行mean、sum或max等pooling操作。

#### Multi-interest Extractor 层

该层的设计是模型重点，输入是用户历史行为，输出是用户的K个兴趣向量，注意这里K是会根据不同用户调整的。

具体内容涉及到 `Capsule` 和 `Dynamic Routing`内容， **留坑**

 

#### Label-aware Attention Layer 层

利用label信息，来判断应该着重哪个兴趣向量，是一个attention操作，label作为query，兴趣向量作为key和value。
$$
\begin{aligned}
\overrightarrow{\boldsymbol{v}}_{u} &=\text { Attention }\left(\overrightarrow{\boldsymbol{e}}_{i}, \mathrm{~V}_{u}, \mathrm{~V}_{u}\right) \\
&=\mathrm{V}_{u} \operatorname{softmax}\left(\operatorname{pow}\left(\mathrm{V}_{u}^{\mathrm{T}} \overrightarrow{\boldsymbol{e}}_{i}, p\right)\right)
\end{aligned}
$$
其中$p$是一个可调超参，当p趋向无穷大时，相当于一个hard attention，实验效果最好。

#### 参考

- [MIND作者分享视频](https://developer.aliyun.com/live/246509)
- [MIND 召回](https://www.yuque.com/caserwin/dbbzw2/vsorgr)

