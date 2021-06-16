2019年阿里淘宝团队的论文，focus在推荐系统召回过程的用户行为序列兴趣挖掘，

- SDM: Sequential Deep Matching Model for Online Large-scale Recommender System

SDM将用户行为进行session化，而不是直接取一系列用户行为进行建模。一个session内可能会有一系列item。文中对session的定义，

- 系统后台标识相同的 Session Id 的，会被SDM 认为是一个Session；
- 两次行为在10分钟以内的会被系统认为是一个 Session；
- 一个Session 长度最长为 50

可以认为最近的session往往表示用户当前的兴趣点，而用户的兴趣一般是多样且多变的，更精准地建模用户兴趣，需要考虑长期行为，即当前session之前的session们（具体时间窗口为一周）。

总结一哈，SDM的学习目标为，给定短期行为（即最近session） $S^u$ 和 长期行为 $L^u$，预测用户感兴趣的item。

#### 整体架构

![pZdezi](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/pZdezi.png)

模型的输入为短期行为 $S^u$ 和 长期行为 $L^u$，分别过神经网络编码器得到对应向量表示 $s_t^u$ 和 $p_u$ ，接着将两者以及用户向量 $e_u$ 进行融合得到最新用户向量表示。训练过程就和Youtube论文里的一样了，sampled softmax+交叉熵。serving阶段也是常规的ANN检索召回topN的item。



#### item、user向量表示

除了id之外，还引入了一些side information。譬如商品类别、店铺、品牌等，
$$
\boldsymbol{e}_{i_{t}^{u}}=\operatorname{concat}\left(\left\{\boldsymbol{e}_{i}^{f} \mid f \in \mathcal{F}\right\}\right)
$$
用户画像同样考虑了多个维度特征，譬如年龄、性别等等
$$
\boldsymbol{e}_{u}=\operatorname{concat}\left(\left\{\boldsymbol{e}_{u}^{p} \mid p \in \mathcal{P}\right\}\right)
$$
![sVVzV6](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/sVVzV6.png)



#### 短期行为兴趣建模

LSTM + multi-head-attention，使用LSTM是出于序列数据考虑，multi-head-attention是出于用户多兴趣点考虑（同时可以消除误点击）。之后再与用户向量做attention

#### 长期行为兴趣建模

正如前文所述，它是最后一个session之前且7天内的用户行为序列，然后拆分成5个序列，分别是 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D_%7Bi+d%7D%5E%7Bu%7D) （用户交互的item序列）、 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D_%7B%5Ctext+%7Bleaf%7D%7D%5E%7Bu%7D) （用户交互的item的leaf category序列）、 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D_%7B%5Ctext+%7Bcate+%7D%7D%5E%7Bu%7D) （用户交互的item的first level category序列）、 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D_%7B%5Ctext+%7Bbrand+%7D%7D%5E%7Bu%7D) （用户交互的brand序列）、 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D_%7B%5Ctext+%7Bshop+%7D%7D%5E%7Bu%7D) （用户交互的shop序列），这些序列分别和 ![[公式]](https://www.zhihu.com/equation?tex=e_%7Bu%7D) 作为AttnNet的输入，得到5个weighted pooling输出，然后把这5个输出拼接，经过一个全连接层，得到 ![[公式]](https://www.zhihu.com/equation?tex=p%5E%7Bu%7D) ，作为用户长期偏好特征。
$$
\begin{aligned}
\alpha_{k}=& \frac{\exp \left(\boldsymbol{g}_{k}^{u T} \boldsymbol{e}_{u}\right)}{\sum_{k=1}^{\left|\mathcal{L}_{f}^{u}\right|} \exp \left(\boldsymbol{g}_{k}^{u T} \boldsymbol{e}_{u}\right)} \\
&\left|\mathcal{L}_{f}^{u}\right| \\
z_{f}^{u}=& \sum_{k=1}^{u} \alpha_{k} \boldsymbol{g}_{k}^{u}
\end{aligned}
$$

$$
\begin{array}{l}
z^{u}=\operatorname{concat}\left(\left\{z_{f}^{u} \mid f \in \mathcal{F}\right\}\right) \\
\boldsymbol{p}^{u}=\tanh \left(\boldsymbol{W}^{p} z^{u}+b\right)
\end{array}
$$

#### 兴趣融合

长短期兴趣融合

1. 计算门控向量

$$
G_{t}^{u}=\operatorname{sigmoid}\left(W^{1} e_{u}+W^{2} s_{t}^{u}+W^{3} p^{u}+b\right)
$$

2. 计算长短期贡献

$$
\boldsymbol{o}_{t}^{\boldsymbol{u}}=\left(1-\boldsymbol{G}_{t}^{u}\right) \odot \boldsymbol{p}^{\boldsymbol{u}}+\boldsymbol{G}_{t}^{u} \odot \boldsymbol{s}_{t}^{u}
$$

#### 一个Trick

使用next T target items（下一次点击的item，以及再下一次点击的item，再再下一次。。。）作为target item，而不是只使用下一次点击的item，SDM在实验中，这种方案效果最佳。主要因为点击行为中可能会出现 skip behaviors现象。具体在下一次Personalized Top-N Sequential Recommendation via Convolutional Sequence Embedding论文中说明。