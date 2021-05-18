来自谷歌 ICML2020 的论文

- SimCLR: A Simple Framework for Contrastive Learning of Visual Representations
- 代码：https://github.com/google-research/simclr

本文的关注点在于无监督视觉表示学习。

#### 整体框架

![uhS2nR](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/uhS2nR.png)

SimCLR主要包括几个部分：

- 数据扩增：由于是图像数据，论文中采用的扩增手段包括，裁剪、缩放、旋转、颜色失真等等。关于数据扩增，作者发现：
  - 单一的某种数据增强并没有很大效果
  - 组合的数据增强中，**随机裁剪+随机颜色表现最突出**(random cropping and random color distortion)，先裁剪，后颜色失真。
  - **相比有监督，对比学习需要更强的数据增强**

![SXxX3f](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/SXxX3f.png)

- 编码器。数据扩增后会变成两路数据，类似双塔模型，共享参数，论文使用的是ResNet + avg-pooling
- 非线性全连接层MLP。主要作用是把双塔出来后的表示映射到相同空间，实验表明有助于改善representation的质量

$$
\boldsymbol{z}_{i}=g\left(\boldsymbol{h}_{i}\right)=W^{(2)} \sigma\left(W^{(1)} \boldsymbol{h}_{i}\right)
$$

- 对比损失。在训练batch中，正样本为数据扩增后的两个，负样本为batch中的其他样本。

$$
\ell_{i, j}=-\log \frac{\exp \left(\operatorname{sim}\left(\boldsymbol{z}_{i}, \boldsymbol{z}_{j}\right) / \tau\right)}{\sum_{k=1}^{2 N} \mathbb{1}_{[k \neq i]} \exp \left(\operatorname{sim}\left(\boldsymbol{z}_{i}, \boldsymbol{z}_{k}\right) / \tau\right)}
$$



![undefined](http://ww1.sinaimg.cn/large/afd47e42ly1gqfpq0xk7ug20g00hsqv5.gif)

实验设置细节就写了，直接看论文吧。最后看一下对比吧！

![80w1hi](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/80w1hi.png)