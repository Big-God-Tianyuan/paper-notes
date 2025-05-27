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
    7. 序列推荐类(Seq)：马尔可夫链模型(FPMC), DL-based (GRU4REC, SASRec, BERT4Rec) (UniRec)
    
    
    9. (不可行)Rule-based
    11. (暂时不考虑)多模态(Multi-modal Fusion): 有字幕信息等的时候 MMGCN, VBPR


已完成：ItemCF, MF, FM, NCF, Popularity-based, Random, LightGCN, KEAM, UniRec


### Step 1: Data

    1. LLM选择：GPT-4o or higher model
    2. Dataset: xuetang, MOOCCube, X (3个？)
    2. small scale data - ablation study 多个LLM 多种prompt

已完成：数据集制作。可用于baseline。LLM生成概念。


### Step 2: Method

    1. baselines
    2. 适配数据的baselines

已完成：baseline 代码
未完成：；**模型扩展**

### Step 3: Experiments

    1. Performance
    2. Ablation
    3. Cold Start

已完成：
未完成：colda start, ablation study?

指标： Precision@K, Recall@K, HR@K, nDCG@K, F1, Accuarcy, MRR

Points:

    1. 流行度的结果好，证明网课吗这个长尾效应更为严重，或者说大多传统推荐算法都会受到数据仅有交互信息带来的局限性，有效的推荐很重要！给交互信息带来side information可以增进推荐效果，然而不是任何时候都可以轻松获得side information。课程名字有语义信息，就可以利用大语言模型生成side information
    2. 数据增强的范式模型，改进各种经典算法，高效利用side information
    3. 推荐性能很受限于数据稀疏度, cold start

## 📌 通用融合方式（适用于多数模型）

| 融合方式         | 说明                                                                 | 技术实现建议 |
|------------------|----------------------------------------------------------------------|----------------|
| **Embedding 拼接** | 将课程ID embedding 和 GPT概念embedding 拼接或加权融合                             | `concat(course_id_emb, concept_emb)`，送入后续模型层 |
| **相似度加权**     | 用 GPT 概念相似度调整 item/item 或 user/item 相似度计算                      | 应用于 CF 类方法（如 ItemCF） |
| **概念交互建模**   | 将概念看作独立特征，与用户进行交叉建模                                         | FM、DeepFM、AutoInt 等结构友好 |
| **注意力机制**     | 引入 attention，动态赋权概念的重要性                                          | 用于 NCF / MLP-based / Transformer-based 模型中 |
| **辅助图结构（非GNN）** | 构建 item-concept 邻接信息用于显式特征生成                                       | 类似 KEAM，用概念关系计算 item-level 先验向量 |



    

### K = 5

|  MOOCCube | UserCF | ItemCF | MF | FM (BPR) | NCF | Popularity | LightGCN | UniRec | KEAM |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|
| Precision@K |  |  |  |  |  |  |  | 0.0995 |  |
| Recall@K |  |  |  |  |  |  |  | 0.0795 |  |
| HR@K |  |  |  |  |  |  |  | 0.4975 |  |
| NDCG@10 |  |  |  |  |  |  |  | 0.1210 |  |
| F1 |  |  |  |  |  |  |  | 0.0884 |  |
| MRR |  |  |  |  |  |  |  | 0.3103 |  |

### K = 10 

|  MOOCCube | UserCF | ItemCF | MF | FM (BPR) | NCF | Popularity | LightGCN | UniRec | KEAM | KEAM+ |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|
| Precision@K | 0.0509 | 0.0344 | 0.0328 | 0.0550 | 0.0471 | 0.0531 | 0.0625 | 0.0660 | 0.0778 | 0.0791 |
| Recall@K | 0.2862 |  0.1890 | 0.1839 | 0.3196 | 0.2637 | 0.2935 | 0.3433 | 0.1044 | 0.4323 | 0.4387 |
| HR@K | 0.4290 | 0.3005 | 0.2923 | 0.4735 | 0.4011 | 0.4323 | 0.5014 | 0.6599 | 0.6033 | 0.6104 |
| NDCG@10 | 0.2088 | 0.1280 | 0.0989 | 0.1559 | 0.1613 | 0.1948 | 0.2372 | 0.1210 | 0.3083 | 0.3150 |
| F1 | 0.0864 | 0.0582 | 0.0556 | 0.0573 | 0.0799 | 0.0900 | 0.1058 | 0.0809 | 0.1319 | 0.1340 |
| Accuarcy | 0.0507 | 0.0343 | 0.1839 | 0.0939 | 0.0471 | 0.0530 | 0.0624 |  | 0.0777 | 0.0790 |
| MRR | 0.2360 | 0.1391 | 0.0914 | 0.1287 | 0.1626 | 0.2065 | 0.2556 | 0.3320 | 0.3327 | 0.3399 |







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

|  MOOCCube | UserCF | ItemCF | MF | FM (BPR) | NCF | Popularity | LightGCN | UniRec | KEAM |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|
| Precision@K |  |  |  |  |  |  |  | 0.0404 |  |
| Recall@K |  |  |  |  |  |  |  | 0.1258 |  |
| HR@K |  |  |  |  |  |  |  | 0.8071 |  |
| NDCG@10 |  |  |  |  |  |  |  | 0.1298 |  |
| F1 |  |  |  |  |  |  |  | 0.0611 |  |
| MRR |  |  |  |  |  |  |  | 0.3424 |  |





## [笔记](https://f0jb1v8xcai.feishu.cn/wiki/DMqOwfhbIi2kUlkxu0RcVaYjn2F)

LLM






## Large Language Models for Recommendation: Progresses and Future Directions 

还有PPT

[https://www.youtube.com/watch?v=zcuOrWxJ2k8](https://www.youtube.com/watch?v=zcuOrWxJ2k8)





## Is ChatGPT a Good Recommender? A Preliminary Study


## Evaluating ChatGPT as a Recommender System: A Rigorous Approach


Try OpenP5- https://yongfeng.me/attach/LLM4RecSys.pdf

    
    
## MOOCCube & MOOCCubeX





