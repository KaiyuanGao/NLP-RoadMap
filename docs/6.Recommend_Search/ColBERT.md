来自论文：Eicient and Eective Passage Search via Contextualized Late Interaction over BERT

### 背景

QD匹配几种经典模型：

- 基于表示：query、doc分别encode，计算相似度（doc端可以离线计算）
- 基于交互：query、doc在底层交互（效果更好，但计算慢）

![image-20210330210400736](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210330210400736.png)

### 模型

提出ColBERT，结合上述两种模型的优势，关键组件：`late interaction mechanism`

- query、doc分别过encoder

$$
\begin{array}{l}
E_{q}:=\text { Normalize }\left(\operatorname{CNN}\left(\operatorname{BERT}\left({ }^{*}[Q] q_{0} q_{1} \ldots q_{l} \# \# \ldots \#^{\prime \prime}\right)\right)\right) \\
E_{d}:=\text { Filter }\left(\text { Normalize }\left(\operatorname{CNN}\left(\operatorname{BERT}\left({ }^{(\alpha}[D] d_{0} d_{1} \ldots d_{n} "\right)\right)\right)\right)
\end{array}
$$

- MaxSim模块：query与doc每个term计算相似度，求max，然后加和

$$
S_{q, d}:=\sum_{i \in\left[\left|E_{q}\right|\right]^{j}} \max _{j\left[\left|E_{d}\right|\right]} E_{q_{i}} \cdot E_{d_{j}}^{T}
$$

![image-20210330210819522](/Users/gaokaiyuan/Library/Application Support/typora-user-images/image-20210330210819522.png)

### 任务

- Top-k Re-ranking

- End-to-end Top-k Retrieval

  