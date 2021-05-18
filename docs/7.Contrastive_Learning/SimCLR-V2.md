来自谷歌Hinton组 NeurIPS 2020 的论文

- Big Self-Supervised Models are Strong Semi-Supervised Learners
- 代码：https://github.com/google-research/simclr

SimCLR-v2主要针对SimCLR-v1做了以下优化：

1. 使用了更大的 ResNet模型，从` ResNet-50 (4×)` 变成更深更窄的 `ResNet-152 (3×+SK)` ，最终预训练该模型后使用 `1%` 的有标注数据进行微调，效果提升 `29%` ；
2. 采用更深的3层MLP，并在迁移到下游任务时保留第一层（以前是完全舍弃），在 `1%` 的监督数据下提升了 `14%`；
3. 参考了MoCo使用memory，但由于batch已经足够大了，只有 `1%` 左右的提升；

本文整体的训练模式为：预训练（大规模任务无关的无标注数据）--> 微调 （少量有标注数据）---> 蒸馏（任务相关的无标注数据）

最终对比效果

![kygHIG](https://cdn.jsdelivr.net/gh/KaiyuanGao/ML-algorithm@master/uPic/kygHIG.png)



