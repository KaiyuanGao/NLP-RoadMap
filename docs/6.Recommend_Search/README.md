## 关于搜索和推荐

自己之前一直是在做NLP相关的，到工作以后才发现NLP简直就是一个『打工仔』，最终应用还得是搜广推，真实too young！这里就记录一些关于工业界在搜索、广告以及推荐领域相关的实践，以及一些论文的阅读笔记，keep reading！

### 领域综述

关于综述首先查看 【[AI-Surveys](https://github.com/KaiyuanGao/AI-Surveys) 】这个项目，里面整理了ML/DL/CV/NLP/Graph等多个AI相关领域的综述。

- [Semantic Matching in Search](http://www.hangli-hl.com/uploads/3/4/4/6/34465961/ml_for_match-step2.pdf)
- [Pretrained Transformers for Text Ranking: BERT and Beyond](https://arxiv.org/pdf/2010.06467.pdf)
- [A Deep Look into Neural Ranking Models for Information Retrieval](https://arxiv.org/pdf/1903.06902.pdf)
- [Pretrained Transformers for Text Ranking: BERT and Beyond](https://arxiv.org/pdf/2010.06467.pdf)
- [An Introduction to Neural Information Retrieval](https://www.microsoft.com/en-us/research/uploads/prod/2017/06/fntir2018-neuralir-mitra.pdf)
- [A Deep Look into neural ranking models for information retrieval](https://arxiv.org/abs/1903.06902) 
- [A Comparison of Supervised Learning to Match Methods for Product Search](https://arxiv.org/abs/2007.10296)
- [深度文本匹配综述](http://cjc.ict.ac.cn/online/onlinepaper/pl-201745181647.pdf)

### 召回

#### 传统模型

- 具体模型以及解读全都整理在：[**【匹配】Learning-to-Match**](https://github.com/KaiyuanGao/NLP-RoadMap/blob/master/docs/4.Natural_Language_Processing/4.4-Text_Matching/[匹配]Learning-to-Match.md)

#### Transformer-based 模型

- 【ICLR 2020】PRE-TRAINING TASKS FOR EMBEDDING-BASED LARGE-SCALE RETRIEVAL.  [[论文](https://arxiv.org/abs/2002.03932)] [[解读]()]
- 【SIGIR 2020】ColBERT: Efficient and Effective Passage Search via Contextualized Late Interaction over BERT. [[论文](https://arxiv.org/abs/2004.12832)] [[笔记](https://github.com/KaiyuanGao/NLP-RoadMap/blob/master/docs/6.Recommend_Search/ColBERT.md)]
- DC-BERT: DECOUPLING QUESTION AND DOCUMENT FOR EFFICIENT CONTEXTUAL ENCODING. [[论文](https://arxiv.org/abs/2002.12591)] [[笔记](https://github.com/KaiyuanGao/NLP-RoadMap/blob/master/docs/6.Recommend_Search/DC-BERT%E7%AC%94%E8%AE%B0.md)]
- 【ICLR 2020】Poly-encoders: Transformer Architectures and Pre-training Strategies for Fast and Accurate Multi-sentence Scoring. [[论文](https://arxiv.org/abs/1905.01969)] [[笔记](https://github.com/KaiyuanGao/NLP-RoadMap/blob/master/docs/6.Recommend_Search/Poly-Encoder笔记.md)]
- 【KDD 20】Embedding-based Retrieval in Facebook Search. [[论文](https://arxiv.org/abs/2006.11632)] [[笔记](https://github.com/KaiyuanGao/NLP-RoadMap/blob/master/docs/6.Recommend_Search/Facebook向量化检索.md)]
- REALM: Retrieval-Augmented Language Model Pre-Training. [[论文](https://arxiv.org/abs/2002.08909)] [[笔记](https://mp.weixin.qq.com/s/6V1HYNPA3qIyKwcQmdRSbw)]
- RepBERT: Contextualized Text Embeddings for First-Stage Retrieval.  [[论文](https://arxiv.org/abs/2006.15498)] [[笔记](https://mp.weixin.qq.com/s/6V1HYNPA3qIyKwcQmdRSbw)]
- RocketQA: An Optimized Training Approach to Dense Passage Retrieval for Open-Domain Question Answering. [[论文](https://arxiv.org/abs/2010.08191)] [[笔记](https://github.com/KaiyuanGao/NLP-RoadMap/blob/master/docs/4.Natural_Language_Processing/4.13-Question_Answering/RocketQA.md)]
- 

### 排序

- [Passage Re-ranking with BERT.](https://arxiv.org/pdf/1901.04085.pdf)
- [Multi-Stage Document Ranking with BERT](https://arxiv.org/pdf/1910.14424.pdf)
- [The Expando-Mono-Duo Design Pattern for Text Ranking with Pretrained Sequence-to-Sequence Models.](https://arxiv.org/pdf/2101.05667.pdf)
- [Simple Applications of BERT for Ad Hoc Document Retrieval](https://arxiv.org/pdf/1903.10972.pdf)
- [Applying BERT to Document Retrieval with Birch](https://www.aclweb.org/anthology/D19-3004.pdf)
- [Cross-Domain Modeling of Sentence-Level Evidence for Document Retrieval](https://www.aclweb.org/anthology/D19-1352.pdf)
- [Deeper Text Understanding for IR with Contextual Neural Language Modeling](https://arxiv.org/pdf/1905.09217.pdf)
- [CEDR: Contextualized Embeddings for Document Ranking](https://arxiv.org/pdf/1904.07094.pdf)
- [Training Curricula for Open Domain Answer Re-Ranking](https://arxiv.org/pdf/2004.14269.pdf)
- [Leveraging Passage-level Cumulative Gain for Document Ranking.](https://dl.acm.org/doi/pdf/10.1145/3366423.3380305) 
- [Selective Weak Supervision for Neural Information Retrieval.](https://arxiv.org/pdf/2001.10382.pdf) 
- [Document Ranking with a Pretrained Sequence-to-Sequence Model.](https://arxiv.org/pdf/2003.06713.pdf)
- [Beyond CLS through Ranking by Generation.](https://arxiv.org/pdf/2010.03073.pdf) 
- [BERT-QE: Contextualized Query Expansion for Document Re-ranking.](https://arxiv.org/pdf/2009.07258.pdf)
- [Cross-lingual Retrieval for Iterative Self-Supervised Training.](https://arxiv.org/pdf/2006.09526.pdf) 
- [Generalizing Discriminative Retrieval Models using Generative Tasks.](https://github.com/Albert-Ma/awesome-pretrained-models-for-information-retrieval/blob/main)
- [PROP: Pre-training with Representative Words Prediction for Ad-hoc Retrieval.](https://arxiv.org/pdf/2010.10137.pdf)
- [Cross-lingual Language Model Pretraining for Retrieval.](https://github.com/Albert-Ma/awesome-pretrained-models-for-information-retrieval/blob/main) 
- [Local Self-Attention over Long Text for Efficient Document Retrieval.](https://arxiv.org/pdf/2005.04908.pdf)
- [The Cascade Transformer: an Application for Efficient Answer Sentence Selection.](https://arxiv.org/pdf/2005.02534.pdf)
- [Using Prior Knowledge to Guide BERT’s Attention in Semantic Textual Matching Tasks.](https://arxiv.org/pdf/2102.10934.pdf)

### 多模态检索

- [Unicoder-VL: A Universal Encoder for Vision and Language by Cross-modal Pre-training.](https://arxiv.org/pdf/1908.06066.pdf) 
- [XGPT: Cross-modal Generative Pre-Training for Image Captioning.](https://arxiv.org/pdf/2003.01473.pdf) 
- [UNITER: UNiversal Image-TExt Representation Learning.](https://arxiv.org/pdf/1909.11740.pdf) 
- [Oscar: Object-Semantics Aligned Pre-training for Vision-Language Tasks.](https://arxiv.org/pdf/2004.06165.pdf) 
- [VinVL: Making Visual Representations Matter in Vision-Language Models.](https://arxiv.org/pdf/2101.00529.pdf)
- [ViLBERT: Pretraining Task-Agnostic Visiolinguistic Representations for Vision-and-Language Tasks.](https://arxiv.org/pdf/1908.02265.pdf)
- [12-in-1: Multi-Task Vision and Language Representation Learning.](https://arxiv.org/pdf/1912.02315.pdf)
- [Learning Transferable Visual Models From Natural Language Supervision.](https://arxiv.org/pdf/2103.00020.pdf) 
- [ERNIE-ViL: Knowledge Enhanced Vision-Language Representations Through Scene Graph.](https://arxiv.org/pdf/2006.16934.pdf)
- [M6-v0: Vision-and-Language Interaction for Multi-modal Pretraining.](https://arxiv.org/pdf/2003.13198.pdf) 
- [M3P: Learning Universal Representations via Multitask Multilingual Multimodal Pre-training.](https://arxiv.org/pdf/2006.02635.pdf) 

### 业界实践分享 

##### 百度

- [MOBIUS：百度凤巢新一代广告召回系统](https://mp.weixin.qq.com/s/2Vr3jQB4RGi2mbIkMMn1mQ)
- [从表示到匹配的语义计算之旅 | ArchSummit](https://www.infoq.cn/video/S5BEM3wyGS3BzqypJ6kE)
- [端到端问答新突破：百度提出RocketQA，登顶MSMARCO榜首](https://mp.weixin.qq.com/s/Oa0fPy4roOveyMU0BsrRhg)

##### 美团

- [Transformer 在美团搜索排序中的实践](https://mp.weixin.qq.com/s/Oixc46P9rQeiMDjI-0j0cw)
- [MT-BERT在文本检索任务中的实践](https://mp.weixin.qq.com/s/5HZULHPI3-HJypvAMXEOcQ)
- [BERT在美团搜索核心排序的探索和实践](https://mp.weixin.qq.com/s/mFRhp9pJRa9yHwqc98FMbg)
- [美团BERT的探索和实践](https://mp.weixin.qq.com/s/qfluRDWfL40E5Lrp5BdhFw)
- [大众点评搜索基于知识图谱的深度学习排序实践](https://tech.meituan.com/2019/01/17/dianping-search-deeplearning.html)

##### 阿里

- [阿里文娱深度语义搜索相关性探索](https://mp.weixin.qq.com/s/1aNd3dxwjCKUJACSq1uF-Q)

- [阿里文娱搜索算法实践与思考](https://mp.weixin.qq.com/s/7hvYdOTnnw5pDDMx6N66Uw)

- [优酷视频元素内容召回系统：多级多模态引擎探索](https://mp.weixin.qq.com/s/XylR3ZHQ_3b15mfcVqGgfg)

- [阿里妈妈深度树匹配技术演进3.0：TDM->JTM->BSAT](https://mp.weixin.qq.com/s/Nd9vCggZ3RfWLMpZ9JRKdQ)

- [阿里深度召回模型实践](https://mp.weixin.qq.com/s/hek-MglfIA4pSkLLLgKfYw)

- [搜索技术：从阿里巴巴搜索业务中台到行业智能搜索云服务的实践](https://yqh.aliyun.com/live/detail/16105)

- [神马搜索如何提升搜索的时效性？](https://mp.weixin.qq.com/s/WpITPvYmixMHa0ha0MgWVA)

- [阿里定向广告新一代主模型：基于搜索的超长用户行为建模范式](https://mp.weixin.qq.com/s/xK6BQVvrqOh79qEP7gtEyA)

- [电商搜索算法技术的演进](https://mp.weixin.qq.com/s/Nj4kVtn41bTyQ04Suc5A3A)

- [阿里巴巴首次揭秘电商知识图谱AliCoCo！淘宝搜索原来这样玩！](https://mp.weixin.qq.com/s/GnEGHMoGJEBVBhhHljqAzA)

  

##### 知乎

- [知乎搜索框背后的Query理解和语义召回技术](https://mp.weixin.qq.com/s/4Ns0qbE9d8KZRjFaSUXvRQ)
- [知乎搜索文本相关性与知识蒸馏](https://mp.weixin.qq.com/s/xgCtgEMRZ1VgzRZWjYIjTQ)

##### 爱奇艺

- [爱奇艺深度语义表示学习的探索与实践](https://mp.weixin.qq.com/s/f524bPx0pq7qxXGjpa7WCQ)
- [爱奇艺搜索排序模型迭代之路](https://mp.weixin.qq.com/s/w-aEwku3LnGdIqYxjq123A)

##### 京东

- [京东电商搜索中的语义检索与商品排序](https://mp.weixin.qq.com/s/4UBehc0eikVqcsFP7xL_Zw)
- [如何构建一个好的电商搜索引擎？](https://mp.weixin.qq.com/s/CAzafDevfNs0hHUmquds2Q)
- [万字长文解读电商搜索——如何让你买得又快又好](https://mp.weixin.qq.com/s/1hc7G4eBSyk-b8Dv4FsYbg)

##### 腾讯

- [搜你所想，从Query意图识别到类目识别的演变](https://mp.weixin.qq.com/s/s8swIdAPw_VeAWnZTL1riA)
- [搜索中的 Query 理解及应用](https://mp.weixin.qq.com/s/rZMtsbMuyGwcy2KU7mzZhQ)

##### Airbnb

- [Airbnb搜索：深度学习排序算法如何进化？](https://mp.weixin.qq.com/s/pAbuPccrZGhF0q0ZBpkL0A)

##### 微软

- [“进化”的搜索方式：揭秘微软语义搜索背后的技术](https://mp.weixin.qq.com/s/5cwn6qI59LakNOwds9VE-w)

### 优质博客

- [Search: Query Matching via Lexical, Graph, and Embedding Methods](https://eugeneyan.com/writing/search-query-matching/)：介绍了几种query匹配的方法，基于词法、基于图、基于embedding等等。
- 

