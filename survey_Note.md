
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

利用图像直觉进行解释，就像图2.1，系统觉得user会喜欢这个lens是因为他的领口外观，所以就圈出来了。(我觉得这个太抽象了)
这个小领域只能说，**未来可期**。

### - Social Explanation

受到"相似用户"这个想法的激发，使用社交关系可以使得用户更加信任系统。像Facebook在推荐新朋友的时候，会和你说共同好友。并且在推荐item中，可以向用户展示她有多少个朋友喜欢该物品。
![social](social.png)

有研究发现，解释会影响user的选择，也就是说更多的朋友在关注的内容，该user也有更大的概率去浏览。**但是**，这个概率却和内容本身的质量无关，就像电影，音乐家这种。

**社交解释和相似用户的解释可以结合使用**

上述为6种解释风格，通常会和下面所讲的推荐算法一起使用。

## 3. Explainable Recommendation Models(Algorithm perspective)

这部分的内容主要介绍如何考虑推荐方法和推荐结果的可解释性。一般的做法为：提高模型**透明度**，这类做法的代表是：Factorization-based, Topic Modeling, Graph-based, Deep Learning, Knowledge-based, rule mining 方法。

另一种做法为：只关注**推荐结果**的可解释性。思想：model看成一个黑盒，并为此开发一个单独的模型用来解释推荐结果。这类做法的代表：post-hoc/model-agnostic方法（事后/模型不可知 模型）

### - Factorization Models for Explainable Recommendation

MF/tensor-factorization 在解释性上问题是user/item embedding是潜在的。在MF中，我们假设user embedding 和 item embedding在低维空间中，每个维度都表示影响user决策的特定因素，但是，传统MF中，无法确定每个因素的确切含义。使得难以解释。

为了解决这个问题：EFM(Explicit Factor Models)，基本思想为推荐用户最喜欢的feature上表现好的item。如图，从review提取item feature然后对齐每个MF的latent dimension和explicit feature，这样就可以获得预测过程来给出解释。

![EFM](EFM.png)

Step1: 从review中进行情感分析，或者一个三元组(Feature, Opinion, Sentiment),(battery, life, -1).

Step2: 把这些特征词作为 Explicit Feature Space，用户对不同特征的关注度和item各个特征的质量被整合到model里。

**EMF细节**：如何将feature加入到MF中。
![EFM2](EFM2.png)

**ONE**：阴影块代表user对item的评论，包括了rating和review text。使用NLP技术识别review中是否含有情感以及积极或消极。再生成(特征，情感分数)的pair。通过这步，我们得到了Feature-Opinion pair。

**TWO**：这篇论文假设用户更倾向于评论他们关心的feature。所以去构建一个matrix X代表user-feature attention。考虑 user<sub>i</sub> 的所有评论，提取所有(Feature, Sentiment)pair。Feature F<sub>j</sub> 被 user<sub>i</sub> 提及 t<sub>ij</sub>次，则在attention matrix X中的表示如下：
![X](EFM_X.png)

就是用sigmoid函数将这个提及频次t<sub>ij</sub>归N化，N为该数据集的理论最大评分，类似于yelp为5分。目的为是的X矩阵和user-item rating matrix A的规格一致。

**Three**：类似于**TWO**，同样构造一个item-feature的质量矩阵 quality matrix来表示item在各个feature上的程度/质量。类似于X矩阵的求法，对于一个物品p<sub>i</sub>,会使用所有对应的评论去提取假设(Feature, Sentiment)pair，Feature F<sub>j</sub> 在 item p<sub>i</sub>上被提及 k次，平均情感为s<sub>ij</sub>。Y矩阵如下可求得：
![X](EFM_Y.png)

这样同时考虑了 流行度 和情感。

**Four**：整合Explicit 和 Implicit Feature,类似于MF分解user-item rating matrix 成2个低维向量，同样在user-feature attention matrix X 和 item-feature quality matrix Y上构造 Factorization model，去估计 latent embedding of users, items, features.可以通过最小化latent embedding的内积与X Y矩阵的差距来实现：
![U1U2](EFM_XY2U.png)

这里 &lambda; 是正则化系数，r是分解得到的**explicit factor**的数量。V代表了特征的向量表达形式(pxr)

user对各个item的评分基于他对item的各个方面综合评价得出，所以这篇论文用U<sub>1</sub>和U<sub>2</sub>分别表示用户对特征的关注，和物品在对应特征上的表现。然而，这些explicit feature不能完全解释用户的评分，所以这篇文章同样引入了implicit feature ( r' 个latent feature),也就是H<sub>1</sub>和H<sub>2</sub>, P Q分别为 U和H的串联，P代表了用户侧，Q代表了物品侧。通过最小化PQ内积与评分矩阵A来获得H：
![PQ](EFM_PQ.png)

hidden factors 的估计估计方法如下：
![min](EFM_min.png)

**注意**：当r = 0时，这就是一个 传统的LFM(latent factorization model)。

整合的Implicit Feature 和 Explicit Feature的示意图如下：我们先得到X Y，然后去做分解得到U V,再引入H，最后计算得到A。
![EFM3](EMF3.png)

通过上述操作，我们可以得到补全后的X', Y', A'矩阵:

- X' = U<sub>1</sub> V<sup>T</sup>
- Y' = U<sub>2</sub> V<sup>T</sup>
- A' = U<sub>1</sub> U<sub>2</sub><sup>T</sup> + H<sub>1</sub> H<sub>2</sub><sup>T</sup>

Top-k推荐：

Explanation：

paper: [*Explicit factor models for explainable recommendation based on phrase-level sentiment analysis*](https://dl.acm.org/doi/10.1145/2600428.2609579)

### - Topic Modeling for Explainable Recommendation

### - Graph-based Models for Explainable Recommendation

### - Deep Learning for Explainable Recommendation

### - Knowledge Graph-based Explainable Recommendation

### - Rule Mining for Explainable Recommendation

### - Model Agnostic and Post Hoc Explainable Recommendation
