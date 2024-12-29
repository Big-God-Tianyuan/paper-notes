# AIED工作思路


主体：基于CELDA2024论文思想：利用ChatGPT生成course concept。进一步提升该工作内容，以及采用更全面的评估手法。

回顾一下：

    KEAM模型(EDM+ICCE) limitation: 没层级关系。可以拓展这个地方在model design。不一视同仁看各个entity。
    CELDA limitation: 只生成了concept信息，并且评估方法比较一般。

可以后续生成relation信息(concept-concept, course-course)， 再用于KEAM的结构。Relation：prerequisite-dependence, similarity.

内容：

    1. concept extraction + evaluation (与Celda相似，提升验证)  one-shot， zero-shot 
    2. relation 先决！ one-shot, zero-shot
    3. concept expansion (AutoKG search)

问题：1.验证手法？  2.可解释性？

1. F1
2. 人


|            | 开放式生成任务(Open-ended Generation Tasks) | 多选任务(Multiple-choice Question Tasks) |
|------------|------------|------------|
| concept 生成 | 直接生成 | List中进行选择 |
| concept 评估 | pre-train model 算概念的相似度？+expert | 一致率(list中有多少被选择出) |
| relation生成 |  | 给Pair concepts 输出relation |
| relation评估 |  | 1 |
| Metircs | 相似分数？ + F1, Recall, Precision | F1, Recall, Precision |

## CELDA2024工作内容：

探索 ChatGPT 在自动生成课程知识概念方面的潜力，并评估其生成概念的准确性和一致性。


## Generating Explanatory Texts on Relationships Between Subjects and Their Positions in a Curriculum using Generative AI

主旨：使用ChatGPT 对课程之间的关系进行分类，并生成解释性文本。

步骤：

    1. 收集 课程信息(如课程名称、授课教师、课程描述等) 和 curriculum信息(课程体系)(学习目标)从 web syllabi 和 curriculum map(九大特色)
    2. 提取 专业术语(learning term)从课程信息(course description, subject plan)，利用TF-IDF(机器学习技术很简单) 计算课程特征的embedding。课程embedding是由这些feature构成，进而计算cos相似度。
    3. 用ChatGPT 分类课程间的关系，并生成text explanation(5类选),再给出被选课程在课程体系中的定位(方法很受局限)。具体如下：把2个课程的课程信息，课程体系信息，还有课程相似度信息给ChatGPT，让GPT从pre-defined的五种关系中选择一个：Foundation-Application1(A → B)，Foundation-Application2(A ← B), Overlap, Parallel(同field但是没关系), Low-Relevance。再次把之前信息和新生成的relation一起喂给GPT让他生成text explanation。
    
    
(NOTE) TF-IDF值: 这个值越大，说明单词在当前文档中重要性越高，但在其他文档中较少出现.

(NOTE) 生成课程在课程体系中的定位方法并不理解，但具体为 先根据 学年 和 被选课程，再结合 相似度选出3个最相似的课程(视为之前修读、当前修读和将来修读的课程)。再用被选课程的描述和9个相似课程的描述给GPT生成定位。

3个实验(部分可借鉴？)：

    1. course relation的Accuracy：先用户实验打标签，再对照一下GPT生成的。算precision，recall，F1 score等
    2. 评估 text explanation。他们生成explanation用了5种promt：Prompt A：仅包含课程描述和课程计划。Prompt B：加入响应模板。Prompt C：加入学习目标。Prompt D：加入关系类型。Prompt E（提议方法）：包含上述所有内容。用户成对对比结果
    3. 对课程定位文本的评价：2方法，比较带有时间顺序和不带时间顺序的课程定位描述。(非常不理解！)




## LLMs for knowledge graph construction and reasoning: recent capabilities and future opportunities (IMPORTANT)

构建KG涉及多个任务：Named Entity Recognition (NER), Relation Extraction (RE), Event Extraction (EE), Entity Linking, Link Prediction (LP), Question Answering (QA).

该论文首先评估了一下LLMs的各个任务的水平，然后设计了一种新颖的虚拟知识提取任务并构建了VINE 数据集。通过评估LLMs在其上的性能，进一步证明了诸如GPT-4之类的LLMs具有很强的泛化能力。最后，引入了自动KG构建和推理的概念，称为**AutoKG**。利用LLMs的内部知识，使LLMs的多个代理能够通过迭代对话协助这一过程。

被划分为三部分：Recent Capabilities, Generalizability Analysis, Future Opportunities.

8个数据集

Recent Capabilities: 初步研究目标是大型语言模型在上述任务中的零样本和单样本能力。该分析用于评估此类模型在知识图领域的潜在用途。实证结果表明，LLMs 像 GPT-4 一样，作为少量信息提取器表现出有限的有效性，但作为推理助手却表现出相当熟练的能力。

Generalizability Analysis: 在新构建的数据集 VINE 上的实验表明，像 GPT-4 这样的大型语言模型可以从指令中获取新知识并有效执行提取任务，从而在一定程度上提供对大型模型更细致的理解。

AutoKG的概念——通过多智能体通信进行自主知识图谱构建和推理。在这个框架中，人类的角色被削弱了，多个交流主体各自扮演着各自的角色。这些代理与外部资源交互，协作完成任务。


**GPT用于knowledge graph reasoning更牛逼** (Link Prediction)



## Core Concept Identification in Educational Resources via Knowledge Graphs and Large Language Models


KG 重要， 自动识别concept同理也重要。

现有局限：

    1. 依赖开放知识图谱， KG一直都得是开放知识图谱的子图。
    2. 标注concept费时费力

该论文使用3个办法 识别课程核心概念：

    1. zero-shot 没任何提示提取三元组。 使用 DBpedia Spotlight（一种消歧和注释服务，将文本中提及的实体链接到 DBpedia 资源）在构建图表之前消除概念歧义。构建图表后，应用 PageRank 来计算资源表示中每个概念（节点）的重要性，从而确定课程的核心概念。
    2. 通过注释课程文本中的 DBpedia 资源来提高模型的性能。有了这些注释，可以指示模型只提取这些注释概念之间的关系。这一修改大大提高了检索课程核心概念的精确度和召回率。
    3. 使用 LLM 生成课程文本的摘要。然后，我们使用 DBpedia Spotlight 对摘要和原文进行注释。摘要和原文中识别出的概念（DBpedia 资源）将提供给 LLM，LLM 现在可以访问摘要、课程文本和这个增强的概念集，以完成三重提取任务。


知识图谱构建：将课程文本语义化表示为一个加权有向图，其中概念作为节点，关系作为边。通过提取三元组（包含源概念、关系和目标概念），并基于概念共现的频率（不考虑具体关系性质）分配权重，构建知识图谱。

Zero-shot：

    将课程文本分块，每块包含约 10-15 句，避免超过模型的上下文窗口。
    每块文本传递给 LLM，指示其提取文本中概念之间的关系，并输出有效的 JSON 格式三元组。
    提取的概念通过 DBpedia Spotlight 进行消歧，未能成功映射的概念保留原始 LLM 提取的文本作为节点。
    通过 PageRank 算法计算节点的重要性，最终识别出课程的核心概念。    

Concept Supported Triple Extraction：

    使用 DBpedia Spotlight 识别课程文本中的实体
    将识别出的实体和文本传递给 LLM，指示其仅提取包含这些实体的关系
    提取的三元组用于构建知识图谱

Expanded Concept List Triple Extraction (解决遗漏相关概念的issue)：

    使用 LLM 对课程文本生成摘要，并通过 DBpedia Spotlight 从生成的摘要中提取新的实体。
    将这些新提取的实体与原始文本中识别的实体结合，传递给 LLM 进行三元组提取。
    结合原始文本和摘要的新概念列表，构建更全面的知识图谱。
    (除了在课程文本和生成摘要中识别的实体外，还要提供课程的原始文本)

评估：

    1. 指标 F1 score; baseline SOTA
    2. LLMs: Claude-3-Opus, GPT-4-0613, GPT-4-Turbo, GPT-4o
    3. 3种构建kg的方法



## What Should I Learn First: Introducing LectureBank for NLP Education and Prerequisite Chain Learning


1. 描述了如何通过神经网络方法和传统分类器来建模先决关系。特别地，我们探索了基于GNN的方法，以学习课程讲义中概念的语义表示，并预测它们的先决关系。

2. 引入了 LectureBank，一个用于 NLP 教育领域的全新数据集，包括 1,352 份讲义、208 个核心主题及其先决关系。


技术GNN，很NLP



## ACE: AI-Assisted Construction of Educational Knowledge Graphs with Prerequisite Relations


结合 ML 和 expert knowledge

引入一种先修关系评分机制，基于单词嵌入（word embeddings）捕获的语义引用对概念对进行评分。随后，这些概念对根据其分数进行排序，得分较高的概念对会被选出供专家评估，从而减少需要评估的概念对总数。又弄了个web

基于先修关系的 EKG 提出一种正式化表达，并提出一种称为 ACE（AI-assisted Construction of Educational KGs，AI辅助构建教育知识图谱），具体说：

    1. 先修关系排序评分机制（Prerequisite Rank Scoring, PRS）：为概念集中的所有概念对分配一个分数，并根据这些分数对它们进行排序，专家仅需对得分较高的概念对进行评估。
    2. 迭代策略：在迭代过程中，系统动态地根据专家对高分概念对的反馈更新 EKG，同时基于当前图谱推断出其他先修关系，从而进一步减少专家的工作量。

ACE 仅使用概念的纯文本描述来推断先修关系，无需依赖外部资源。

先修关系评分机制基于文本描述中的引用，这些引用可以是精确引用或语义引用


limitation: 太局限于文档文本了

实验做得很牛逼，有点晕晕的


## Course Concept Expansion in MOOCs with External Knowledge and Interactive Game

技术太难了 很NLP。学学写作 学学他的概念和找prerequisite concept的方法？

RQ: 解决 语义漂移

提出个 **扩展概念(Expanded Concept)** ：课程材料中没有直接提及，但与课程内容相关，并对学习有帮助的概念。
(NOTE) 不同于先修概念(Prerequisite Concept)

课程中教授的概念外，还有许多相关概念也值得学习。多数研究没考虑课程概念扩展。

Challenge:

    1. 语义漂移：与明确类别（例如“国家”）的集合扩展不同，课程概念通常是多类别的组合。在跨领域探索中（例如结构：堆、二叉树；算法：分治、贪心），容易导致语义漂移(Semantic Drift)。
    2. 信息异质性：课程相关概念的特征是异质的。例如，堆（Heap）因其与二叉搜索树的相似结构被视为课程概念，而二叉搜索树又是Tango树的前置概念。因此，仅依靠上下文信息不足以有效扩展概念。
    3. 引入人类反馈的难度：作为一个面向应用的任务，如何合理利用MOOC用户的反馈来优化概念扩展的表现仍然是一个具有挑战性的问题。

提出了一种三阶段的课程概念扩展模型。首先为给定课程构建一个准确的边界，以减少从外部知识库生成候选概念时的语义漂移。然后，将扩展过程转化为一个二分类问题，类似于之前用于集合扩展的正例-未标注学习（Positive-Unlabeled Learning）方法。该论文提出三种特征(置信分数特征（Confidence Features）、搜索路径编码特征（Search Path Encoding Features）和先修特征（Prerequisite Features）)，结合异质信息，通过分类器识别高质量候选概念。

三阶段：候选概念生成(动态边界)，概念分类，基于游戏的优化。

概念分类里的先修特征衡量：两个概念的一些相互指标：共现频率（Co-occurrence Frequency），路径长度（Path Length），关系类型（Relation Type），上下文一致性（Contextual Consistency），前后顺序分布（Relative Order Distribution）



## Course Concept Extraction in MOOCs via Embedding-Based Graph Propagation

good writing (RQ为低频概念咋办！)

MOOCs 内容中自动提取课程概念是一项重要的研究任务，因为Concept 可以帮助学生更好理解课程内容，且不同背景的学生需要的concept不同。 有的概念对于无相关背景的人来说，我们需要标识 对应的先修概念，反之则不必。弄concept 是既费时又费力还得靠老师，现有的技术在MOOC上还是不太行。最重要的就是低频concept问题:

    1.课程视频字幕是相对较短的文档，词数较少；
    2.许多不常见的课程概念来自其他前置或相关课程（例如，“均匀分布”来自数学课程，而“分治法”来自算法课程）；
    3.一个明确的课程概念往往以多种方式表达，从而产生许多分散的低频术语。例如，“Q sort”是快速排序的俗语表达，“partition exchange sort”是快速排序的另一个名称。它们在课程视频中都很少出现。


该论文结合在线百科全书为候选课程概念学习潜在表示(类似 word embedding)。此外，提出了一种新的基于图的传播算法(Embedding-Based Graph Propagation (基于嵌入的图传播方法))，用于根据学到的表示对候选概念进行排名。方法很有意思(NLP+GNN的感觉)

验证手法：

    1. 双人工标注，并使用pearson correlation 评估一致性，只有当两位标注者对某候选概念的判断一致时，该候选概念才被标注为课程概念。
    2. 评估指标：R-precision（Rp），平均准确率（Mean Average Precision, MAP）



## Prerequisite Relation Learning for Concepts in MOOCs 

good writing


提出一个机器学习方法 预测MOOCs 中知识概念之间的先修关系，利用**语义信息，上下文特征，结构特征**3个方面的信息，进行综合建模。再找各种分类器做实验。

涉及的feature：

    1. Semantic Relatedness
    2. Video Reference Distance, Sentence Reference Distance, Wikipedia Reference Distance
    3. Average Position Distance, Distributional Asymmetry Distance, Complexity Level Distance

方法有点老，且不好复现。但提供了一个小数据，可以用来后续的**comparison**. (GPT生成一份，看看差距)

验证过程：人工标签后，把这些概念embedding 送进分类器预测。(不是很好去拿过来用。没法这么弄GPT生成的概念)



## Measuring prerequisite relations among concepts

当学习概念A时，如果需要大量引用与A相关的概念中的B，而B很少引用到A，那么B很可能是A的先决条件，而非相反。将概念建模为一个向量空间，使用其相关概念来表示，并通过计算两个概念相关内容的引用差异（参考距离，Reference Distance，**RefD**）来度量它们的先决条件关系。

RefD由该论文提出，测量A的相关概念引用B的频次。有3性质：归一化，不对称性，反身性。




## Knowledge Graph-Based Core Concept Identification in Learning Resources


基于KG从学习资源中自动识别核心概念。这种方法通过语义表示将学习资源中的文本和知识图谱中的概念关联起来，并实现了基于无监督权重策略和监督机器学习方法的核心概念提取。

利用文本中的概念提及，结合知识图谱中的背景信息，生成加权有向图.图的节点表示概念，边表示概念间的语义关系.通过类别扩展（基于分类层级的扩展）和属性扩展（基于知识图谱中的属性）引入新概念



## AI-Augmented Advising: A Comparative Study of ChatGPT-4 and Advisor-based Major Recommendations

也许可以看一下这篇论文 怎么用embedding 算相似度的 evaluation 的方法可以参考

Evaluation:

    RQ1: AI的专业推荐、推荐理由和问题回答与顾问的相似性如何？
    RQ2: 包含学生的人口统计信息是否会提升AI建议的表现？
    RQ3: 顾问是否会因查看AI建议而改变自己的建议？

论文采用了一个预训练语言模型来生成文本的语义嵌入，用于量化AI和顾问生成内容之间的语义相似性。

## Foundation metrics for evaluating effectiveness of conversations powered by generative AI (看看指标)




## ACUTE-EVAL: Improved Dialogue Evaluation with Optimized Questions and Multi-turn Comparisons (想想评估)



## Large Language Models for Recommendation: Progresses and Future Direction (AIED后边的工作思路)

还有PPT

[https://www.youtube.com/watch?v=zcuOrWxJ2k8](https://www.youtube.com/watch?v=zcuOrWxJ2k8)


LLMs 推荐系统 [笔记](https://f0jb1v8xcai.feishu.cn/wiki/DMqOwfhbIi2kUlkxu0RcVaYjn2F)

## MOOCCube & MOOCCubeX





