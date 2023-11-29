
# Explainable Recommendation A Survey and New Perspectives

## 1. Introduction

### - Historical Overview

RS早期方法主要集中在CF和Content-based methods。
CF：推荐user的item主要是与user之前交互过的item set里相似的item。CF的核心思想为为user推荐和他相似的users交互的物品。也就是群体智慧来预测缺失的评分。但CF的解释性不如Content-based。CF与LFM(latent factor model)结合时，**MF**效果甚佳。但是！MF核心思路为降维，这很难解释。为了理解模型，提出了EFM(Explicit Factor Model),使得latent dimension与explicit feature对齐进行解释。

Content-based：基于各种attribute 推荐、价格，颜色，品牌；导演，类型，时长。因为Content-based RS 很容易被理解，可以通过content提供**Explaination**

### - Classification

2个正交维度进行分类：

    1. HCI角度: The information source or display style of the explanations(文本句子或视觉解释)
    2. 算法角度: The model to generate explainations(KNN, MF, topic modeling, DL, GNN, KG, association rule mining...)
Figure

### - Explainability and Interpretability

XAI目的时开发让用户理解的模型的决策。为了实现目标，模型可以是 interpretable or non-interpretable。 Interpretable model 致力于 局部或者全局透明的模型(transparent)

