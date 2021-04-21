## 关于搜索和推荐

自己之前一直是在做NLP相关的，到工作以后才发现NLP简直就是一个『打工仔』，最终应用还得是搜广推，真实too young！这里就记录一些关于工业界在搜索、广告以及推荐领域相关的实践，以及一些论文的阅读笔记，keep reading！

### 领域综述

关于综述首先查看 【[AI-Surveys](https://github.com/KaiyuanGao/AI-Surveys) 】这个项目，里面整理了ML/DL/CV/NLP/Graph等多个AI相关领域的综述。

- [Semantic Matching in Search](http://www.hangli-hl.com/uploads/3/4/4/6/34465961/ml_for_match-step2.pdf)
- [Pretrained Transformers for Text Ranking: BERT and Beyond](https://arxiv.org/pdf/2010.06467.pdf)

- [A Deep Look into Neural Ranking Models for Information Retrieval](https://arxiv.org/pdf/1903.06902.pdf)
- [Pretrained Transformers for Text Ranking: BERT and Beyond](https://arxiv.org/pdf/2010.06467.pdf)
- [An Introduction to Neural Information Retrieval](https://www.microsoft.com/en-us/research/uploads/prod/2017/06/fntir2018-neuralir-mitra.pdf)
- [深度文本匹配综述](http://cjc.ict.ac.cn/online/onlinepaper/pl-201745181647.pdf)
- [A Deep Look into neural ranking models for information retrieval](https://arxiv.org/abs/1903.06902) 
- [A Comparison of Supervised Learning to Match Methods for Product Search](https://arxiv.org/abs/2007.10296)

### 召回

#### 传统模型

- 具体模型以及解读全都整理在：[**【匹配】Learning-to-Match**](https://github.com/KaiyuanGao/NLP-RoadMap/blob/master/docs/4.Natural_Language_Processing/4.4-Text_Matching/[匹配]Learning-to-Match.md)

##### Transformer-based 模型

- 【ICLR 2020】PRE-TRAINING TASKS FOR EMBEDDING-BASED LARGE-SCALE RETRIEVAL.  [[论文](https://arxiv.org/abs/2002.03932)] [[解读]()]
- 【SIGIR 2020】ColBERT: Efficient and Effective Passage Search via Contextualized Late Interaction over BERT. [[论文](https://arxiv.org/abs/2004.12832)] [[笔记](https://github.com/KaiyuanGao/NLP-RoadMap/blob/master/docs/6.Recommend_Search/ColBERT.md)]
- DC-BERT: DECOUPLING QUESTION AND DOCUMENT FOR EFFICIENT CONTEXTUAL ENCODING. [[论文](https://arxiv.org/abs/2002.12591)] [[笔记](https://github.com/KaiyuanGao/NLP-RoadMap/blob/master/docs/6.Recommend_Search/DC-BERT%E7%AC%94%E8%AE%B0.md)]
- 【ICLR 2020】Poly-encoders: Transformer Architectures and Pre-training Strategies for Fast and Accurate Multi-sentence Scoring. [[论文](https://arxiv.org/abs/1905.01969)] [[笔记](https://github.com/KaiyuanGao/NLP-RoadMap/blob/master/docs/6.Recommend_Search/Poly-Encoder笔记.md)]
- 【KDD 20】Embedding-based Retrieval in Facebook Search. [[论文](https://arxiv.org/abs/2006.11632)] [[笔记](https://github.com/KaiyuanGao/NLP-RoadMap/blob/master/docs/6.Recommend_Search/Facebook向量化检索.md)]

### 排序

- 

### 多模态检索

- 

### 业界实践分享 

- [MOBIUS：百度凤巢新一代广告召回系统](https://mp.weixin.qq.com/s/2Vr3jQB4RGi2mbIkMMn1mQ)
- [从表示到匹配的语义计算之旅 | ArchSummit](https://www.infoq.cn/video/S5BEM3wyGS3BzqypJ6kE)
- [端到端问答新突破：百度提出RocketQA，登顶MSMARCO榜首](https://mp.weixin.qq.com/s/Oa0fPy4roOveyMU0BsrRhg)
- [Transformer 在美团搜索排序中的实践](https://mp.weixin.qq.com/s/Oixc46P9rQeiMDjI-0j0cw)
- [MT-BERT在文本检索任务中的实践](https://mp.weixin.qq.com/s/5HZULHPI3-HJypvAMXEOcQ)
- [BERT在美团搜索核心排序的探索和实践](https://mp.weixin.qq.com/s/mFRhp9pJRa9yHwqc98FMbg)
- [美团BERT的探索和实践](https://mp.weixin.qq.com/s/qfluRDWfL40E5Lrp5BdhFw)
- [大众点评搜索基于知识图谱的深度学习排序实践](https://tech.meituan.com/2019/01/17/dianping-search-deeplearning.html)
- [阿里文娱深度语义搜索相关性探索](https://mp.weixin.qq.com/s/1aNd3dxwjCKUJACSq1uF-Q)
- [阿里文娱搜索算法实践与思考](https://mp.weixin.qq.com/s/7hvYdOTnnw5pDDMx6N66Uw)
- [阿里妈妈深度树匹配技术演进3.0：TDM->JTM->BSAT](https://mp.weixin.qq.com/s/Nd9vCggZ3RfWLMpZ9JRKdQ)
- [阿里深度召回模型实践](https://mp.weixin.qq.com/s/hek-MglfIA4pSkLLLgKfYw)
- [知乎搜索框背后的Query理解和语义召回技术](https://mp.weixin.qq.com/s/4Ns0qbE9d8KZRjFaSUXvRQ)
- [知乎搜索文本相关性与知识蒸馏](https://mp.weixin.qq.com/s/xgCtgEMRZ1VgzRZWjYIjTQ)
- [爱奇艺深度语义表示学习的探索与实践](https://mp.weixin.qq.com/s/f524bPx0pq7qxXGjpa7WCQ)
- [爱奇艺搜索排序模型迭代之路](https://mp.weixin.qq.com/s/w-aEwku3LnGdIqYxjq123A)
- [京东电商搜索中的语义检索与商品排序](https://mp.weixin.qq.com/s/4UBehc0eikVqcsFP7xL_Zw)
- [如何构建一个好的电商搜索引擎？](https://mp.weixin.qq.com/s/CAzafDevfNs0hHUmquds2Q)
- [万字长文解读电商搜索——如何让你买得又快又好](https://mp.weixin.qq.com/s/1hc7G4eBSyk-b8Dv4FsYbg)
- [搜你所想，从Query意图识别到类目识别的演变](https://mp.weixin.qq.com/s/s8swIdAPw_VeAWnZTL1riA)
- [搜索中的 Query 理解及应用](https://mp.weixin.qq.com/s/rZMtsbMuyGwcy2KU7mzZhQ)
- [Airbnb搜索：深度学习排序算法如何进化？](https://mp.weixin.qq.com/s/pAbuPccrZGhF0q0ZBpkL0A)

