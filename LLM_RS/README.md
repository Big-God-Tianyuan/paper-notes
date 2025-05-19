# LLM-based CRS工作思路

## (Current Work)利用LLM-generated concepts 进行数据增强的CRS

利用AIED工作中生成的课程概念，进行数据增强。用各种推荐算法进行测试，并关注冷启动。

可能的推荐算法：

    1. 协同过滤类：CF (UserCF ≈ UserKNN, ItemCF ≈ ItemKNN)
    2. 矩阵分解类：MF, BPR <Implicit MF (ALS)>
    3. FM FFM
    5. Graph-based: 图嵌入模型Graph Embedding (Node2Vec/DeepWalk), 图卷积模型GCN (NGCF, LightGCN)
    6. 深度学习类：NCF (Neural Collaborative Filtering)
    8. 知识图谱类：RippleNet, KGAT
    10. Popularity-based, Random

    4. Contend-based: TF-IDF/BM25, Doc2Vec/BERT Embedding-based model,  Content-based Filtering (CBF)
    7. 序列推荐类(Seq)：马尔可夫链模型(FPMC), DL-based (GRU4REC, SASRec)
    9. Rule-based
    
    
    11. (暂时不考虑)多模态(Multi-modal Fusion): 有字幕信息等的时候 MMGCN, VBPR


已完成：ItemCF, MF, FM, NCF, Popularity-based, Random
未完成：NGCF, LightGCN, KEAM

### Step 1: Data

    1. LLM选择：GPT-4o or higher model
    2. Dataset: xuetang, MOOCCube, X (3个？)
    2. small scale data - ablation study 多个LLM 多种prompt

已完成：数据集制作。可用于baseline。
未完成：LLM生成概念。

### Step 2: Method

    1. baselines
    2. 适配数据的baselines

已完成：
未完成：baseline 代码；模型改写

### Step 3: Experiments

    1. Performance
    2. Ablation
    3. Cold Start

已完成：
未完成：

指标： Precision@K, Recall@K, HR@K, nDCG@K, F1, Accuarcy, MRR



### K = 5

### K = 10 

|  MOOCCube | UserCF | ItemCF | MF | FM (BPR) | NCF | Popularity |  |
|------------|------------|------------|------------|------------|------------|------------|------------|
| Precision@K | 0.0509 | 0.0344 | 0.0328 | 0.0550 | 0.0471 | 0.0531 |   |
| Recall@K | 0.2862 |  0.1890 | 0.1839 | 0.3196 | 0.2637 | 0.2935 |   |
| HR@K | 0.4290 | 0.3005 | 0.2923 | 0.4735 | 0.4011 | 0.4323 |   |
| NDCG@10 | 0.2088 | 0.1280 | 0.0989 | 0.1559 | 0.1613 | 0.1948 |   |
| F1 | 0.0864 | 0.0582 | 0.0556 | 0.0573 | 0.0799 | 0.0900 |   |
| Accuarcy | 0.0507 | 0.0343 | 0.1839 | 0.0939 | 0.0471 | 0.0530 |   |
| MRR | 0.2360 | 0.1391 | 0.0914 | 0.1287 | 0.1626 | 0.2065 |   |





| X | UserCF | ItemCF |
|------------|------------|------------|
| Precision@K |  |  |
| Recall@K |  |  |
| HR@K |  |  |
| F1 |  |  |
| Accuarcy |  |  |
| MRR |  |  |


|  XuetangX | UserCF | ItemCF |
|------------|------------|------------|
| Precision@K |  |  |
| Recall@K |  |  |
| HR@K |  |  |
| F1 |  |  |
| Accuarcy |  |  |
| MRR |  |  |


### K = 15

### K = 20


## [笔记](https://f0jb1v8xcai.feishu.cn/wiki/DMqOwfhbIi2kUlkxu0RcVaYjn2F)

LLM






## Large Language Models for Recommendation: Progresses and Future Directions 

还有PPT

[https://www.youtube.com/watch?v=zcuOrWxJ2k8](https://www.youtube.com/watch?v=zcuOrWxJ2k8)





## Is ChatGPT a Good Recommender? A Preliminary Study


## Evaluating ChatGPT as a Recommender System: A Rigorous Approach


Try OpenP5- https://yongfeng.me/attach/LLM4RecSys.pdf

    
    
## MOOCCube & MOOCCubeX





