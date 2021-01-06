#  NLP-RoadMap

## 介绍（Introduction）

The last thing you figure out in writing something is what to put first.



## 数理基础（Mathematic）

###  线性代数

https://en.wikipedia.org/wiki/Linear_algebra

-  标量、向量、张量
-  范数：L1/L2/L0
-  行列式
- 矩阵

     	- 特征分解

     	  https://zh.wikipedia.org/wiki/%E7%89%B9%E5%BE%81%E5%88%86%E8%A7%A3

     - 奇异值分解

       https://zh.wikipedia.org/wiki/%E5%A5%87%E5%BC%82%E5%80%BC%E5%88%86%E8%A7%A3

     - 矩阵求导

       https://www.zhihu.com/question/39523290

       https://pan.baidu.com/s/1uoZOo8El7vH-6yHTZWfUMw

     - Moore-Penrose伪逆

- 线性变换

  https://zh.wikipedia.org/wiki/%E7%BA%BF%E6%80%A7%E6%98%A0%E5%B0%84

- 常见的距离

  https://zhuanlan.zhihu.com/p/58819850

   -  曼哈顿距离
  -  欧式距离
  -  闵可夫斯基距离
  -  切比雪夫距离
  -  余弦距离
  -  汉明距离
  -  杰卡德相似系数
  -  杰卡德距离

###  微积分

https://zh.wikipedia.org/wiki/%E5%BE%AE%E7%A7%AF%E5%88%86%E5%AD%A6

- 导数（如反向传播链式求导）
- 极限
- 极值
- 梯度

### 概率统计

https://zh.wikipedia.org/wiki/%E6%A6%82%E7%8E%87%E8%AE%BA

https://zh.wikipedia.org/wiki/%E7%BB%9F%E8%AE%A1%E5%AD%A6

-  期望、方差、协方差
-  常见的分布

    -  0-1分布
   -  几何分布
   -  二项式分布
   -  高斯分布
   -  指数分布
   -  泊松分布

-  Lagrange乘子法
-  最大似然估计

### 最优化理论

https://zh.wikipedia.org/wiki/%E6%9C%80%E4%BC%98%E5%8C%96

- 凸集、凸函数、超平面、半空间
- 梯度下降
- 牛顿法、阻尼牛顿法、拟牛顿法

###  信息论

https://www.zhihu.com/question/37349649/answer/1582736375

- 熵、条件熵
- 互信息、点互信息
- 最大熵模型
-  KL散度

### 推荐资料

-  MIT Gilbert Strang 线性代数 课程

  https://www.bilibili.com/video/av883067591/

-  《线性代数及其应用》教材
-  Essence of calculus

  https://www.youtube.com/playlist?list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr

-  Essence of linear algebra

  https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab

-  陈希儒《概率论与数理统计》
-  Mathematics for Machine Learning

  https://mml-book.github.io/

- MIT的概率系统分析与应用概率

  https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-041-probabilistic-systems-analysis-and-applied-probability-fall-2010/

-  斯坦福的Convex optimization 

  教材：https://web.stanford.edu/~boyd/cvxbook/bv_cvxbook.pdf
  课程：https://www.youtube.com/watch?v=McLq1hEq3UY

-  Numerical optimization  

  教材：http://www.bioinfo.org.cn/~wangchao/maa/Numerical_Optimization.pdf
  课程：http://www.cs.nthu.edu.tw/~cherung/teaching/2009cs5321/index.html

## 编程基础（Coding）

### 编程语言

https://www.zhihu.com/question/284549387/answer/451046449

https://www.zhihu.com/question/23743892/answer/25873078

-  python
- C++
- Java

###  数据结构

https://www.zhihu.com/question/21318658

- 线性表
     - 数组
     - 动态数组
     - 栈
     - 队列
       - 普通队列
       - 并发队列
       - 双端队列
       - 阻塞队列
     - 散列表
       - 散列函数
       - 冲突
       - 位图
       - 动态扩容
     - 链表
       - 单链表
       - 双向链表
       - 循环链表

- 树
    - 二叉树
      - 二叉查找书

      - 平衡二叉树
      - 平衡二叉查找树
      - AVL树
      - 红黑树
      - 完全二叉树

     - 多路查找树
       - B树

       - B+树
       - 2-3树
       - 2-3-4树

     - 堆
          - 大顶堆
           - 小顶堆

     - Trie树

- 图

	- 邻接矩阵、领接表
	- 最小生成树
	- 最短路径

###  算法

- 基本算法思想

	- 动态规划
	- 回溯
	- 分治
	- 贪心

- 查找

     - 二分查找
     - DFS
     - BFS
     - 双指针
     - 滑动窗口

- 排序

     	- O(N^2)
     - 冒泡排序
        		 - 选择排序
              	- 希尔排序
           	- 插入排序
     
     - O(nlogn)

           - 归并排序
          - 堆排序
          - 快速排序

     - O(n+k)

           - 计数排序
          - 基数排序
          - 桶排序

- 字符串匹配

     - 暴力法
     - BM
     - KMP
     - Trie

- 手撕算法

     	- 面试必考，一起刷题吧，公众号NewBeeNLP刷题群 等你

### 智力题

### 推荐资料

-  Coursera上的C++程序设计

  https://www.coursera.org/learn/cpp-chengxu-sheji

- 圣经： C++ prime
- 普林斯顿 Algorithm

  https://www.coursera.org/learn/algorithms-part1

- 《算法导论》
- 可视化学习数据结构网站

  http://www.comp.nus.edu.sg/~stevenha/visualization/index.html

- Leetcode、 剑指 offer、编程珠玑、编程之美

## 机器学习（Machine Learning）

### 数据

Updating 

### 模型

#### 线性回归

- 监督模型、判别模型、回归模型
- 问题

-  求解

	- 最小二乘法 --> 解析解

	- 梯度下降法 --> 数值解

	- 两种解法各自优缺点

	   - 解析解 无需选择learning rate alpha，无需循环，坏处是要有复杂的矩阵运算，复杂度比较高O(n^3)
  - 梯度下降则是O(d*n^2)
	
- 正则项

  在一般的线性回归的基础上加入了正则项，在保证最佳拟合误差的同时，使得参数尽可能的“简单”，使得模型的泛化能力强
  
- 岭回归

  在常规的线性回归模型加上L2正则


- Lasso回归

  在常规的线性回归模型加上L1正则

#### 感知机

感知机是二分类的线性分类模型，属于判别模型。
模型的输入为实例的特征向量，模型的输出为实例的类别：正类取值 +1, 负类取值  -1  。
感知机的物理意义：将输入空间（特征空间）划分为正、负两类的分离超平面。

 -  有监督模型、判别模型、分类模型
-  问题

-  损失函数

  -  

-  几何解释

 -  分离超平面

-  算法

 -  原始形式
  - 对偶形式

-  感知机收敛性定理

#### 逻辑回归

关于逻辑回归，面试官们都怎么问：https://mp.weixin.qq.com/s/Mdn9yiT20oFhyuFLyd6YnA

- 有监督模型、判别模型、分类模型 

- 问题

  Logistic 回归的本质是：假设数据服从这个分布，然后使用极大似然估计做参数的估计

- 损失函数
	
	- 交叉熵损失
	
- 求解
																		
  最好自己手推实现
  
  -  梯度下降法
  -  （拟）牛顿法
	- 优缺点
	
	  优点：
	  （模型）模型清晰，背后的概率推导经得住推敲。
	  （输出）输出值自然地落在0到1之间，并且有概率意义（逻辑回归的输出是概率么？https://www.jianshu.com/p/a8d6b40da0cf）。
	  （参数）参数代表每个特征对输出的影响，可解释性强。
	  （简单高效）实施简单，非常高效（计算量小、存储占用低），可以在大数据场景中使用。
	  （可扩展）可以使用online learning的方式更新轻松更新参数，不需要重新训练整个模型。
	  （过拟合）解决过拟合的方法很多，如L1、L2正则化。
	  （多重共线性）L2正则化就可以解决多重共线性问题。
	
	  缺点：
	  （特征相关情况）因为它本质上是一个线性的分类器，所以处理不好特征之间相关的情况。
	  （特征空间）特征空间很大时，性能不好。
	  （精度）容易欠拟合，精度不高。

​		与其他模型的对比

#### SVM

-  有监督、判别、分类/回归 模型
- 线性可分SVM

  假设训练数据集是线性可分的，则学习的目标是在特征空间中找到一个分离超平面，能将实例分到不同的类。
  
  - 硬间隔最大化
  	- 最优化目标
  -  
  	
  - 对偶算法
  
  	- 拉格朗日函数
  	-  对偶算法的优点
  
- 线性SVM
	
	  -  软间隔最大化
	-  最优化目标
	
	 -  
	
	-  对偶问题
	
	 -  KKT条件
	
	-  合叶损失函数
	
- 非线性SVM
	
	  -  软间隔最大化 + 核技巧
	-  最优化目标
	  -  
	
	-  核函数
	  -  定义
	   -  常用核函数
	    -  多项式
	     -  高斯
	    -  sigmoid
	
- 支持向量回归
	
	  -  原始问题
	-  
	
	-  对偶问题
	
	 -  KKT条件 
	
	-  损失函数
	
-  SVDD
	
	- 序列最小最优化
	
	  支持向量机的学习问题可以形式化为求解凸二次规划问题。这样的凸二次规划问题具有全局最优解，并且有多种算法可以用于这一问题的求解。
	
	    当训练样本容量非常大时，这些算法往往非常低效。而序列最小最优化(sequential minimal optimization:SMO）算法可以高效求解。


​	

- SMO算法
- 讨论
  - SVM与其他ML模型的对比
  - SVM优缺点

#### 朴素贝叶斯 

朴素贝叶斯法是基于贝叶斯定理与特征条件独立假设的分类方法。


对给定的训练集：
首先基于特征条件独立假设学习输入、输出的联合概率分布。
然后基于此模型，对给定的输入  ，利用贝叶斯定理求出后验概率最大的输出  。

 - 有监督、生成式、分类模型 

 - 一些定义

   - 特征条件独立假设
- 贝叶斯定理
   - 先验、后验概率
- 极大似然估计
   - 贝叶斯估计

  	-  损失函数

  - 一些讨论

      	-  NB模型的优缺点
            	- 与其他模型的对啵
       - 半朴素贝叶斯分类器

#### KNN

- 有监督、判别式、分类/回归模型

- lazy learning、非参数学习算法

- K值选择

- 距离度量方法

- KD树

- 优缺点

#### 决策树

决策树的学习目标是：根据给定的训练数据集构造一个决策树模型，使得它能够对样本进行正确的分类。

决策树学习通常包括3个步骤：
	  特征选择。
  	决策树生成。
  	决策树剪枝。

- 有监督、判别式、分类/回归模型
- 特征选择

  特征选择的关键是：选取对训练数据有较强分类能力的特征。若一个特征的分类结果与随机分类的结果没有什么差别，则称这个特征是没有分类能力的
  
  - 信息熵相关理论
   - 信息增益
  - 信息增益比

-  生成算法
	
  决策树生成算法：

	  构建根结点：将所有训练数据放在根结点。
	  
	  选择一个最优特征，根据这个特征将训练数据分割成子集，使得各个子集有一个在当前条件下最好的分类。
	  
	  若这些子集已能够被基本正确分类，则将该子集构成叶结点。
	  若某个子集不能够被基本正确分类，则对该子集选择新的最优的特征，继续对该子集进行分割，构建相应的结点。
	  如此递归下去，直至所有训练数据子集都被基本正确分类，或者没有合适的特征为止。
	​
	​		- ID3
	​		- C4.5
	​		- 两者对比

- 剪枝算法
																		
  生成的决策树可能对训练数据有很好的分类能力，但是对于未知的测试数据却未必有很好要的分类能力，即可能发生过拟合的现象。

	  解决的方法是：对生成的决策树进行剪枝，从而使得决策树具有更好的泛化能力。
	  剪枝是去掉过于细分的叶结点，使得该叶结点中的子集回退到父结点或更高层次的结点并让其成为叶结点。
	  
		- 叶子节点数量
	- 预剪枝
		- 后剪枝
	
	- CART树
	
	  CART: classfification and regression tree ：学习在给定输入随机变量  条件下，输出随机变量  的条件概率分布的模型。
	  
		- 生成
	
		- CART 分类树
	
			- 基尼指数
	
		- CART回归树
	
			- 平方误差
	
	- 剪枝
	
		- 多数表决法
			- 平均法
	
	- 一些讨论
	
	- 优缺点
		- 与其他模型对比
	- 连续值、缺失值处理

#### 集成学习模型

集成学习ensemble learning是通过构建并结合多个学习器来完成学习任务。其一般结构为：
先产生一组“个体学习器”（individual learner) 。个体学习器通常由一种或者多种现有的学习算法从训练数据中产生。

如果个体学习器都是从某一种学习算法从训练数据中产生，则称这样的集成学习是同质的homogenerous。


此时的个体学习器也称作基学习器base learner，相应的学习算法称作基学习算法。

如果个体学习器是从某几种学习算法从训练数据中产生，则称这样的集成学习是异质的heterogenous 。

再使用某种策略将它们结合起来。集成学习通过将多个学习器进行组合，通常可以获得比单一学习器显著优越的泛化性能。



 - Boosting

      个体学习器之间存在强依赖关系、必须串行生成的序列化方法，每一轮迭代产生一个个体学习器

     - AdaBoost
     
      	  具体内容 待更新 
     - GBDT
     
      	  具体内容 待更新
     	- XGBoost
     
      	  具体内容 待更新     
      	- LightGBM
    
      	  具体内容 待更新
     
- Bagging
  
        个体学习器之间不存在强依赖关系、可同时生成的并行化方法
    
   - 随机森林
     
     具体内容 待更新
   
- 集成策略
      	- 平均法
      - 投票法
      - 学习法
- 多样性分析
- 一些讨论
   - 优缺点
	- 模型对比

#### 概率图模型

- HMM

- CRF

#### 降维

- 维度灾难
- 线性判别分析（LDA）

	  基本思想：
	  训练时：给定训练样本集，设法将样例投影到某一条直线上，使得同类样例的投影点尽可能接近、异类样例的投影点尽可能远离。要学习的就是这样的一条直线。
	  预测时：对新样本进行分类时，将其投影到学到的直线上，在根据投影点的位置来确定新样本的类别。
	  
	- 有监督模型
		- 建模
	
		- 
			- 类内散度矩阵
			- 类间散度矩阵
			-  广义瑞利商
	
	- 求解
	
		- 拉格朗日乘子法

- 主成分分析PCA

 - 核化线性降维 KPCA

 - 流形学习

    流形学习manifold learning是一类借鉴了拓扑流形概念的降维方法。 

- 独立成分分析

- t-SNE
	- LargeVis
	
#### 聚类

	- 性能指标
	
		- 外部指标
	
			-  Jaccard系数
			- FM指数
			- Rand指数
			- ARI指数
	
		- 内部指标
	
			-  DB指数
			- Dunn指数 
	
	- 原型聚类
	- 密度聚类
	- 层次聚类
	- 谱聚类

### 训练

### 保存

## 深度学习（Deep Learning）

### NN网络

https://zh.wikipedia.org/wiki/%E4%BA%BA%E5%B7%A5%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C


- DNN

	-  输入层、隐层、输出层
	- 反向传播算法

	  反向传播（back-propagation）指的是计算神经网络参数梯度的方法。总的来说，反向传播依据微积分中的链式法则，沿着从输出层到输入层的顺序，依次计算并存储目标函数有关神经网络各层的中间变量以及参数的梯度。
	  
	  ！！！不要嫌麻烦， 最好自己手推实现一遍
	
- 梯度消失 & 爆炸
																		
  
	- https://zhuanlan.zhihu.com/p/33006526
	
		- 产生原因

		  本质上是因为神经网络的更新方法，梯度消失是因为反向传播过程中对梯度的求解会产生sigmoid导数和参数的连乘，sigmoid导数的最大值为0.25，权重一般初始都在0，1之间，乘积小于1，多层的话就会有多个小于1的值连乘，导致靠近输入层的梯度几乎为0，得不到更新。梯度爆炸是也是同样的原因，只是如果初始权重大于1，或者更大一些，多个大于1的值连乘，将会很大或溢出，导致梯度更新过大，模型无法收敛。
	
	
	​	  
	​	
	
	- 解决方法
		
			- 预训练加微调
		- 梯度裁剪
			- 权重正则
			- 换激活函数
			- 加BN层
			- 使用残差结构
	
- CNN

  卷积神经网络convolutional neural network:CNN：是指那些至少在网络的某一层中使用了卷积运算来代替一般的矩阵乘法运算的神经网络。
  
	- 网络组件

		- 卷积层

	  - 稀疏交互

	
	    	  卷积层通过使用核矩阵来实现稀疏交互（也称作稀疏连接，或者稀疏权重），每个输出单元仅仅与少量的输入单元产生关联。
	    	  
	    	  这降低了网络的参数和计算量，不仅减少了模型的存储需求，也降低了计算复杂度。
	
	
    ​	  
	
    	- 参数共享
			
			  参数共享：在模型的多个位置使用相同的参数。
			  传统的全连接层中，权重矩阵不同位置处的参数相互独立。
			  卷积层中，同一个核会在输入的不同区域做卷积运算。
			  核函数会在输入的不同区域之间共享，这些区域的大小就是核函数的大小。
			  物理意义：将小的、局部区域上的相同的线性变换（由核函数描述），应用到输入的很多区域上。

	
    	- 等变表示
			
			  如果一个函数满足：输入发生改变，输出也以同样的方式改变，则称它是等变的equivariant 。
			  
			  对于卷积层，它具有平移等变的性质：如果  是输入的任何一个平移函数（如：将图片向右移动一个像素），则下面的两个操作的结果是相同的：
			  先对图片进行卷积之后，再对结果应用平移函数  
			  先对图片应用平移函数  ，再对图片进行卷积

	
    -  池化层
	
	    	- max-pooling
	        	- avg-pooling
    	-  平移近似不变性
	
    - 填充 & 步长
	
-  参数计算
	
	  https://cs231n.github.io/convolutional-networks/
  
	- 前向、反向传播

	- 相关变体

		- 空洞卷积
		- 1*1 卷积
		- 拼接卷积
		-  反卷积
		- 分组卷积
		- 非对称卷积核
		- 多尺寸卷积

- RNN

  循环神经网络是一种共享参数的网络：参数在每个时间点上共享。
  
  传统的前馈神经网络在每个时间点上分配一个独立的参数，因此网络需要学习每个时间点上的权重。而循环神经网络在每个时间点上共享相同的权重。
  
  

	-  参数计算
	-  BPTT

	- Teacher forcing 算法
	-  长程依赖

		- 梯度消失 & 爆炸

		  https://zhuanlan.zhihu.com/p/76772734
		  
		  https://www.zhihu.com/question/34878706
		  

	- 相关变体

		- 双向RNN
		- 深度RNN
		- LSTM

			- 门控结构
			- 前向、反向传播
			- 参数计算

		- GRU

	- 讨论

		- 优缺点
		
	- 与CNN的区别
	
- Transformer

	- 结构：encoder-decoder

		- attention
		- FFN
		- embedding
		- Mask操作
		- Add & Norm

	- 与RNN/CNN的区别
	- 并行化
	- 魔改Transofrmer

- Auto-Encoder
- GAN

### 激活函数

激活函数网上的解读比较多，这里暂时不细致整理

后面有空再说吧~

- sigmoid
- softmax
- tanh
- relu
- gelu

### 损失函数

关于损失函数，网上也非常多的整理，善用搜索引擎

https://zhuanlan.zhihu.com/p/77686118


- 分类


   - 0-1损失
  - hinge 损失
  - sigmoid损失
  - 交叉熵损失

- 回归


   - 均方差损失 MSE
  - 绝对值损失
  - Huber 损失
  - smooth L1 损失

### 优化器

https://kaiyuan.blog.csdn.net/article/details/80303279


- SGD
- Momentum
- Adagrad
- Adadelta
- RMSProp
- Adam
- AdaMax
- Nesterov accelerated gradient

### 初始化

https://kaiyuan.blog.csdn.net/article/details/89736837

- 参数初始化要求
- 全零初始化
- 随机初始化
-  Xavier初始化方法
- Kaiming初始化（又称He / MSRA初始化）

###  正则化

https://zh.wikipedia.org/wiki/%E6%AD%A3%E5%88%99%E5%8C%96_(%E6%95%B0%E5%AD%A6)


- 数据方面


      - 增加数据
     	- label smoothing

- 结构方面 


      - Normalization
     - Batch Normalization
      - Layer Normalization
      - Instance Normalization
      - Group Normalization
     - Dropout
     - weight decay
     - L1 正则
     - L2 正则

- 训练方面


      - early stop
     	- warmup

### 深度框架

- Caffe
- Tensorflow
- Pytorch
- Keras
- Paddle
- mxnet
- OneFlow

### 讨论

-  分布式训练常用技术

##  自然语言处理（Natural Language Processing）

### 基础 

- Tokenization

   - Word-based
  - Char-Based
  - SubWord

- Embedding

   - One-Hot
  - 静态向量
  - 动态向量

- 常用Encoder

   - FC
  - CNN
  - RNN
  - Attention
  - Transformer
  - 讨论

- 常用Decoder

   - Beam-Search
  - Greedy-Search
  - Top-K sampling
  - Top-P sampling

### 任务 & 模型

- 文本分类

- 情感分析
- 文本匹配

  文本匹配归结成给定两个文本，判定两个文本之间的关系，转化为分类问题或者排序问题
  
  	- 自然语言推理
  	
  	  给定一个前提条件（premise），判断另一个假设（hypothesis）与之相融/矛盾/无关
  
  - 释义识别（Paraphrase Identification）
  
    给定一对文本，判断是否是对同一个含义的不同表达
  
  - 文本检索（Ad-hoc Retrieval）
  																		
    根据待检索词（query terms）判断与被检索文档（documents）的相关性


- 序列标注


   - 分词
  - 词性标注
  - 命名实体识别
  - 关键词抽取
  - 词义角色标注

- 阅读理解
- 智能问答
- 信息抽取
- 文本生成


   - 机器翻译
  - 摘要生成
  - 对话生成
  - 问答
  - 文本风格转换

- 语言模型

	- N-gram
	
- 子主题 2
	
- 预训练模型

### 应用

- 搜索
- 推荐
- 知识图谱

### 推荐资料

