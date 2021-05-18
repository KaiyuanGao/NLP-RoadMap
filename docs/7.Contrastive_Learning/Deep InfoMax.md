来自ICLR 2019 的论文

- Learning Deep Representations by Mutual Information Estimation and Maximization
- 代码：https://github.com/rdevon/DIM
- 分享视频：https://www.youtube.com/watch?v=o1HIkn8LEsw&t=256s

无监督学习一个重要的问题就是学习有用的 representation，本文的目的就是训练一个 representation learning 函数（即编码器encoder） ，其通过最大编码器输入和输出之间的互信息(MI)来学习对下游任务有用的 representation，而互信息可以通过 MINE [1][1]的方法进行估算。



首先来看基础的图像编码器：

![P9C5rofLEXB81Mc](https://i.loli.net/2021/05/12/P9C5rofLEXB81Mc.png)



#### DIM-v1

基础版本的DIM，用图片 encode 后得到的 representation（$y$），与同一张图片的 feature map 组成正样本对，和另一张图片的 feature map 组成负样本对，然后利用 GAN 的思想训练一个 Discriminator 来区分这两种样本对

![uC7ui3](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/uC7ui3.png)

#### DIM-V2

作者提到，直接最大化全部输入和编码器的输出（即全局的MI）更适合于重建性的任务，而在分类的下游任务上效果不太好。而最大化输入的局部区域（例如不是完整的图片，而是图片中的一块）和输出（即学到的representation）的平均互信息在下游任务（如图像分类）上的效果更好。

具体而言，任务改为判断完整图片与图片的某一部分是否相似，正样本来自同一张图片的局部特征，负样本来自不同图片的局部特征，模型如下

![qPr0B3](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/qPr0B3.png)

