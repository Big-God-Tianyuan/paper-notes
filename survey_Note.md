
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
这是一个survey中的一个分类图
![分类图](classification.png)

### - Explainability and Interpretability

XAI目的时开发让用户理解的模型的决策。为了实现目标，模型可以是 interpretable or non-interpretable。 Interpretable model 致力于局部或者全局透明的模型(transparent)，类似于决策树模型，线性模型，path-depend。这些都是易于向用户说明推荐的原因。**但是**，interpretability并不是实现EX的唯一办法。可以用过一些技术去揭示模型内部决策机制。e.g., attention mechanism, NLP技术, post-hoc解释模型(事后解释)。

## 2. Information Source/Display Style(HCI perspective)

这个标题意味着，向用户展示一条信息，告诉用户为什么推荐特定项目。有着很多的不同风格的解释，如图所示是各种不同风格的解释。
![不同种类的解释](diff_types.png)

### - User/Item-based Explanation

提到用户或者物品-based就会想到CF，user-based or item-based explanations 通常利用implict/explicit feedback 来解释推荐。首先为用户找到相似用户(邻居)，然后邻居们对推荐的item有着积极的评分 -> 推荐。可以使用图表的形式向目标用户展示，进行解释。如图所示，
![CF_EX](CF_EX.png)
邻居觉得它很好，所以推荐了。Item-based就会告诉用户，推荐的item和之前user喜欢的item有一些相似。

previous research说提供解释有诸多好处：**transparency(透明性)，scrutability(可审查性),可信性，有效性，persuasiveness(说服性)，效率，满意度**

User-bansed EX 和 Item-based EX比较， Item EX更易于理解，因为用户了解他之前交互的内容，但用户对别的相似用户可能并不知道，所以说服力有所下降。解决办法：不是基于相似用户，而是基于社交联系。是的用户信任感增加，并且解释会变得更易于接受。

### - Feature-based Explanation

类似于content-based，feature-baseed就是基于内容做出解释，像电影的导演，类型，演员等。如图为对目标用户推荐的解释，推荐的电影的各个属性，系统觉得用户会感兴趣。
![Feature](Feature.png)
当然解释风格不局限于此，还有雷达图(5角星)来展示解释。
Feature还包括了user profile,like 人口统计特征 年龄性别居住地...

### - Opinion-based Explanation

被分为：aspect-level and sentence-level方法(方面级，句子级)。
aspect-level：展示item aspect(color, quality)和对应分数做为解释。
sentence-level：展示解释的句子。

aspect-level 解释类似于Feature-based，不同处为：一般aspect在item或者user profile中不可以直接使用，而是需要从item review去学习对应观点和情绪。类似于从review中抽出“方面-意见-情感”的三元组，"噪音-高-负面"，“屏幕-清晰-正面”。
这些三元组可以通过词云的形式去解释。

### - Sentence Explanation

sentence-level就是为用户提供解释的句子，被分为2种：template-based and generation-based 方法。
基于模板的就是把feature填到准备好的句子里.
![template](template.png)
也有研究使用regression tree来进行选择解释的语句。

generation-based模型：不使用模板而是直接生成解释语句。我们可以使用LSTM生成解释，还可以结合crowdsourcing/GRU门电路去生成解释。这种模型极度依赖review，但需要注意的是review里有很多噪音。

### - Visual Explanation

### - Social Explanation
