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

已完成：baseline 代码。扩展：CF| KEAM | MF| FM |
未完成：**模型扩展**： NCF | LightGCN | UniRec 

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
| Precision@K |  | 0.0525(0.0610) | 0.0341(0.0753) | 0.0492(0.0751) | 0.0609 |  | 0.0893 | 0.0995 |  |
| Recall@K |  | 0.1464(0.1725) | 0.0973(0.2104) | 0.1442(0.2097) | 0.1719 |  | 0.2470 | 0.0795 |  |
| HR@K |  | 0.2326(0.2767) | 0.1617(0.3251) | 0.2304(0.324487) | 0.2763 |  | 0.3782 | 0.4975 |  |
| NDCG@10 |  | 0.1118(0.1324) | 0.0655(0.1642) | 0.0950(0.163866) | 0.1269 |  | 0.2012 | 0.1210 |  |
| F1 |  | 0.0773(0.0901) | 0.0505(0.1109) | 0.0733(0.110536) | 0.0899 |  | 0.1312 | 0.0884 |  |
| MRR |  | 0.1299(0.1553) | 0.0724(0.1930) | 0.1016(0.192892) | 0.1464 |  | 0.2393 | 0.3103 |  |

 



FM               Precision@5  Recall@5      HR@5    NDCG@5     MRR@5      F1@5
One-hot           0.004338  0.010782  0.021130  0.008078  0.010567  0.006187
Sign-Discrete     0.070057  0.193673  0.312105  0.154654  0.187265  0.102895
Cluster           0.068178  0.190548  0.303682  0.140698  0.161811  0.100424
FM+DNN            0.075052  0.209667  0.324487  0.163866  0.192892  0.110536

MF+ Linear 模型评估结果:

    Precision@5: 0.0457
    Recall@5: 0.1193
    HR@5: 0.2033
    NDCG@5: 0.0852
    MRR: 0.0994
    F1: 0.0661

MF+ Attention 模型评估结果:

    Precision@5: 0.0412
    Recall@5: 0.1050
    HR@5: 0.1857
    NDCG@5: 0.0741
    MRR: 0.0870
    F1: 0.0592

MF+ Concat 模型评估结果:

    Precision@5: 0.0753
    Recall@5: 0.2104
    HR@5: 0.3251
    NDCG@5: 0.1642
    MRR: 0.1930
    F1: 0.1109

MF+ Gated 模型评估结果:
    Precision@5: 0.0451
    Recall@5: 0.1164
    HR@5: 0.1984
    NDCG@5: 0.0834
    MRR: 0.0974
    F1: 0.0651


### K = 10 

|  MOOCCube | UserCF | ItemCF (+相似度) | MF (+concat) | FM/BPR (DNN) | NCF() | Popularity | LightGCN() | UniRec | KEAM | KEAM+ |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|
| Precision@K | 0.0509 | 0.0344 (0.0472) | 0.0328 (0.0524) | 0.0419(0.0531) | 0.0469(0.0511) | 0.0531 | 0.0625(0.0767) | 0.0660 | 0.0778 | 0.0791 |
| Recall@K | 0.2862 |  0.1890 (0.2631) | 0.1839 (0.2890) | 0.2395(0.2932) | 0.2622(0.2835) | 0.2935 | 0.3433(0.4227) | 0.1044 | 0.4323 | 0.4387 |
| HR@K | 0.4290 | 0.3005 (0.4018) | 0.2923 (0.4258)  | 0.3651(0.4320) | 0.3978(0.4269) | 0.4323 | 0.5014(0.5948) | 0.6599 | 0.6033 | 0.6104 |
| NDCG@10 | 0.2088 | 0.1280 (0.1663) | 0.0989 (0.1935) | 0.1319(0.1947) | 0.1608(0.1759) | 0.1948 | 0.2372(0.2883) | 0.1210 | 0.3083 | 0.3150 |
| F1 | 0.0864 | 0.0582 (0.0801) | 0.0556 (0.0887) | 0.0713(0.0899)  | 0.0796(0.0865) | 0.0900 | 0.1058(0.1298) | 0.0809 | 0.1319 | 0.1340 |
| MRR | 0.2360 | 0.1391 (0.1717) | 0.0914 (0.2059) | 0.1209(0.2065)  | 0.1622(0.1792) | 0.2065 | 0.2556(0.3024) | 0.3320 | 0.3327 | 0.3399 |




模型                       
LightGCN+ Linear: 
    Precision@10: 0.0738
    Recall@10: 0.4079
    HR@10: 0.5792
    NDCG@10: 0.2804
    MRR: 0.2977
    Accuracy: 0.0737
    F1: 0.1250
   
LightGCN+ Attention:
    Precision@10: 0.0748
    Recall@10: 0.4124
    HR@10: 0.5824
    NDCG@10: 0.2832
    MRR: 0.2996
    Accuracy: 0.0747
    F1: 0.1267

LightGCN+ Concat:
    Precision@10: 0.0764
    Recall@10: 0.4195
    HR@10: 0.5926
    NDCG@10: 0.2863
    MRR: 0.3011
    Accuracy: 0.0763
    F1: 0.1293  

LightGCN+ Gated:
    Precision@10: 0.0767
    Recall@10: 0.4227
    HR@10: 0.5948
    NDCG@10: 0.2883
    MRR: 0.3024
    Accuracy: 0.0765
    F1: 0.1298
    
LightGCN+ Graph-enhanced:
    Precision@10: 0.0615
    Recall@10: 0.3370
    HR@10: 0.4936
    NDCG@10: 0.2334
    MRR: 0.2519
    Accuracy: 0.0614
    F1: 0.1040



LightGCN+Attention 在不同k值下的评估结果:
                 5         10        15        20
Precision  0.107169  0.074839  0.059401  0.050072
Recall     0.297984  0.412416   0.48664  0.543982
HR         0.446776  0.582393  0.659909  0.714936
NDCG        0.24027  0.283175  0.306514   0.32267
MRR        0.281453  0.299555  0.305654   0.30876
F1         0.157643  0.126688  0.105878  0.091704



LightGCN+Concat 在不同k值下的评估结果:
                 5         10        15        20
Precision  0.108712  0.076411  0.060904  0.051178
Recall     0.302235  0.419534  0.498312  0.555335
HR         0.454016  0.592559  0.673533  0.727673
NDCG       0.242063  0.286309  0.310983  0.327027
MRR        0.282605  0.301101  0.307491  0.310532
F1         0.159907  0.129277  0.108542  0.093719



LightGCN+GraphEnhance 在不同k值下的评估结果:
                 5         10        15        20
Precision  0.087535  0.060308  0.049042  0.042041
Recall     0.242088  0.330345  0.400381  0.455886
HR         0.370855  0.483717  0.566848  0.626869
NDCG       0.198087  0.231209  0.253183  0.268729
MRR        0.236634  0.251545  0.258113  0.261489
F1         0.128578  0.101996   0.08738  0.076984



LightGCN+Gated 在不同k值下的评估结果:
                 5         10        15        20
Precision  0.109888  0.076674  0.060935  0.051274
Recall     0.307055  0.422703   0.49995  0.557795
HR         0.460163  0.594775   0.67439  0.729151
NDCG       0.244698  0.288279  0.312523  0.328792
MRR        0.284423  0.302376  0.308671  0.311753
F1         0.161853  0.129803  0.108631  0.093915



LightGCN+Linear 在不同k值下的评估结果:
                 5         10        15        20
Precision  0.107217  0.073843  0.058963      0.05
Recall     0.298968  0.407914  0.485965  0.546174
HR         0.449967  0.579201  0.661121  0.718039
NDCG       0.239559  0.280413   0.30478  0.321711
MRR        0.280521  0.297698  0.304158  0.307367
F1         0.157832  0.125049  0.105166  0.091613






NCF_LinearFusion:

    Top-5 主要指标:
    Precision@5: 0.0659
    Recall@5: 0.1856
    HR@5: 0.2969
    NDCG@5: 0.1357
    MRR: 0.1557
    F1: 0.0972
    
    Top-10 主要指标:
    Precision@10: 0.0504
    Recall@10: 0.2804
    HR@10: 0.4224
    NDCG@10: 0.1714
    MRR: 0.1724
    F1: 0.0855
    
    Top-15 主要指标:
    Precision@15: 0.0415
    Recall@15: 0.3443
    HR@15: 0.5014
    NDCG@15: 0.1913
    MRR: 0.1786
    F1: 0.0741
    
    Top-20 主要指标:
    Precision@20: 0.0361
    Recall@20: 0.3967
    HR@20: 0.5623
    NDCG@20: 0.2060
    MRR: 0.1820
    F1: 0.0662



NCF_AttentionFusion:

    Top-5 主要指标:
    Precision@5: 0.0660
    Recall@5: 0.1857
    HR@5: 0.2984
    NDCG@5: 0.1390
    MRR: 0.1620
    F1: 0.0974
    
    Top-10 主要指标:
    Precision@10: 0.0511
    Recall@10: 0.2835
    HR@10: 0.4269
    NDCG@10: 0.1759
    MRR: 0.1792
    F1: 0.0865
    
    Top-15 主要指标:
    Precision@15: 0.0425
    Recall@15: 0.3519
    HR@15: 0.5105
    NDCG@15: 0.1973
    MRR: 0.1858
    F1: 0.0758
    
    Top-20 主要指标:
    Precision@20: 0.0368
    Recall@20: 0.4035
    HR@20: 0.5705
    NDCG@20: 0.2118
    MRR: 0.1892
    F1: 0.0675



NCF_GatedFusion:

    Top-5 主要指标:
    Precision@5: 0.0662
    Recall@5: 0.1862
    HR@5: 0.2989
    NDCG@5: 0.1413
    MRR: 0.1658
    F1: 0.0977
    
    Top-10 主要指标:
    Precision@10: 0.0511
    Recall@10: 0.2845
    HR@10: 0.4262
    NDCG@10: 0.1783
    MRR: 0.1828
    F1: 0.0867
    
    Top-15 主要指标:
    Precision@15: 0.0419
    Recall@15: 0.3470
    HR@15: 0.5038
    NDCG@15: 0.1978
    MRR: 0.1888
    F1: 0.0747
    
    Top-20 主要指标:
    Precision@20: 0.0364
    Recall@20: 0.4002
    HR@20: 0.5670
    NDCG@20: 0.2127
    MRR: 0.1924
    F1: 0.0668



NCF_DirectConcatFusion:

    Top-5 主要指标:
    Precision@5: 0.0613
    Recall@5: 0.1731
    HR@5: 0.2776
    NDCG@5: 0.1289
    MRR: 0.1492
    F1: 0.0906
    
    Top-10 主要指标:
    Precision@10: 0.0475
    Recall@10: 0.2647
    HR@10: 0.4016
    NDCG@10: 0.1636
    MRR: 0.1658
    F1: 0.0805
    
    Top-15 主要指标:
    Precision@15: 0.0395
    Recall@15: 0.3294
    HR@15: 0.4815
    NDCG@15: 0.1836
    MRR: 0.1721
    F1: 0.0706
    
    Top-20 主要指标:
    Precision@20: 0.0340
    Recall@20: 0.3758
    HR@20: 0.5369
    NDCG@20: 0.1966
    MRR: 0.1752
    F1: 0.0623








模型             Precision@10	Recall@10   	HR@10       	NDCG@10     	MRR         	F1          
MF+ Linear     0.0394      	0.2069      	0.3267      	0.1183      	0.1160      	0.0662      
MF+ Attention  0.0402      	0.2086      	0.3329      	0.1139      	0.1075      	0.0675      
MF+ Concat     0.0524      	0.2890      	0.4258      	0.1935      	0.2059      	0.0887      
MF+ Gated      0.0384      	0.1998      	0.3159      	0.1147      	0.1129      	0.0644 


模型(FM)            Precision@10  Recall@10     HR@10   NDCG@10    MRR@10     F1@10
One-hot            0.026591   0.145413  0.242213  0.056900  0.037281  0.044961
Sign-Discrete      0.050739   0.280670  0.417667  0.187904  0.202128  0.085941
Cluster            0.050981   0.283616  0.420770  0.175039  0.176869  0.086427
FM+DNN             0.053062   0.293183  0.432000  0.194694  0.206460  0.089860


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


|  MOOCCube | ItemCF | MF | FM (BPR) | NCF | Popularity | LightGCN | KEAM |
|------------|------------|------------|------------|------------|------------|------------|------------|
| Precision@K | 0.0290(0.0401) | 0.0308(0.0425) | 0.0358(0.042650) | 0.0393 |  | 0.0504  |  |
| Recall@K | 0.2376(0.3323)  | 0.2565(0.3506) | 0.3033(0.351956) | 0.3282 |  | 0.4115  |  |
| HR@K | 0.3666(0.4873) | 0.3915(0.5012) | 0.4475(0.501478) | 0.4767 |  |  0.5809 |  |
| NDCG@10 | 0.1432(0.1878) | 0.1222(0.2122) | 0.1471(0.212907) | 0.1816 |  | 0.2586  |  |
| F1 | 0.0516(0.0716) | 0.0550(0.0757) | 0.0641(0.076081) | 0.0701 |  | 0.0898 |  |
| MRR | 0.1444(0.1784) | 0.1002(0.2112) | 0.1189(0.211850) | 0.1690 |  | 0.2618 |  |






 FM              Precision@15  Recall@15     HR@15   NDCG@15    MRR@15     F1@15
One-hot            0.031414   0.257561  0.400969  0.111762  0.085355  0.055999
Sign-Discrete      0.041549   0.343436  0.493321  0.207157  0.206910  0.074130
Cluster            0.041395   0.340887  0.491223  0.209265  0.209875  0.073826
FM+DNN             0.042650   0.351956  0.501478  0.212907  0.211850  0.076081


MF+ Linear 模型评估结果:

    Precision@15: 0.0342
    Recall@15: 0.2692
    HR@15: 0.4049
    NDCG@15: 0.1359
    MRR: 0.1186
    F1: 0.0608

MF+ Attention 模型评估结果:

    Precision@15: 0.0389
    Recall@15: 0.3059
    HR@15: 0.4561
    NDCG@15: 0.1481
    MRR: 0.1240
    F1: 0.0690

MF+ Concat 模型评估结果:

    Precision@15: 0.0425
    Recall@15: 0.3506
    HR@15: 0.5012
    NDCG@15: 0.2122
    MRR: 0.2112
    F1: 0.0757

MF+ Gated 模型评估结果:

    Precision@15: 0.0338
    Recall@15: 0.2652
    HR@15: 0.3978
    NDCG@15: 0.1378
    MRR: 0.1236
    F1: 0.0599


### K = 20

|  MOOCCube | UserCF | ItemCF | MF | FM (BPR) | NCF | Popularity | LightGCN | UniRec | KEAM |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|
| Precision@K | | 0.0237(0.0353)  | 0.0279(0.0361) | 0.0315(0.036023) | 0.0340 |  |  | 0.0404 |  |
| Recall@K |  | 0.2570(0.3869) | 0.3093(0.3955) | 0.3510(0.394309) | 0.3763 |  |  | 0.1258 |  |
| HR@K |  | 0.3958(0.5499) | 0.4587(0.5516) | 0.5073(0.551983) | 0.5345 |  |  | 0.8071 |  |
| NDCG@10 |  | 0.1488(0.2031) | 0.1343(0.2237) | 0.1635(0.224326) | 0.1945 |  |  | 0.1298 |  |
| F1 |  | 0.0435(0.0647) | 0.0512(0.0662) | 0.0578(0.066015) | 0.0624 |  |  | 0.0611 |  |
| MRR |  | 0.1460(0.1819) | 0.1000(0.2120) | 0.1280(0.213912) | 0.1712 |  |  | 0.3424 |  |





FM             Precision@20  Recall@20     HR@20   NDCG@20    MRR@20     F1@20
One-hot            0.027147   0.296128  0.442875  0.108617  0.063607  0.049735
Sign-Discrete      0.035312   0.384995  0.543413  0.219385  0.210260  0.064691
Cluster            0.035383   0.385605  0.546398  0.219410  0.210055  0.064819
FM+DNN             0.036023   0.394309  0.551983  0.224326  0.213912  0.066015



MF+ Linear 模型评估结果:

    Precision@20: 0.0303
    Recall@20: 0.3169
    HR@20: 0.4636
    NDCG@20: 0.1495
    MRR: 0.1235
    F1: 0.0553

MF+ Attention 模型评估结果:

    Precision@20: 0.0328
    Recall@20: 0.3421
    HR@20: 0.4928
    NDCG@20: 0.1470
    MRR: 0.1078
    F1: 0.0599

MF+ Concat 模型评估结果:

    Precision@20: 0.0361
    Recall@20: 0.3955
    HR@20: 0.5516
    NDCG@20: 0.2237
    MRR: 0.2120
    F1: 0.0662

MF+ Gated 模型评估结果:

    Precision@20: 0.0305
    Recall@20: 0.3177
    HR@20: 0.4635
    NDCG@20: 0.1515
    MRR: 0.1258
    F1: 0.0556


## [笔记](https://f0jb1v8xcai.feishu.cn/wiki/DMqOwfhbIi2kUlkxu0RcVaYjn2F)

LLM






## Large Language Models for Recommendation: Progresses and Future Directions 

还有PPT

[https://www.youtube.com/watch?v=zcuOrWxJ2k8](https://www.youtube.com/watch?v=zcuOrWxJ2k8)





## Is ChatGPT a Good Recommender? A Preliminary Study


## Evaluating ChatGPT as a Recommender System: A Rigorous Approach


Try OpenP5- https://yongfeng.me/attach/LLM4RecSys.pdf

    
    
## MOOCCube & MOOCCubeX





