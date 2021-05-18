来自论文

- Improved Baselines with Momentum Contrastive Learning
- 代码：https://github.com/facebookresearch/moco

Moco-V2，是参考SimCLR 对V1版本进行改进的，效果超越了SimCLR，果然是取其精华。。。

具体而言，参考了SimCLR的 `非线性MLP` 、 `数据扩增` 以及 `余弦学习率`。效果如下

![B42RtD](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/B42RtD.png)

和SimCLR的对比

![gWcprr](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/gWcprr.png)

