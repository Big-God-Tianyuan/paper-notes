# AIED工作思路


主体：基于CELDA2024论文思想：利用ChatGPT生成course concept。进一步提升该工作内容，以及采用更全面的评估手法。

回顾一下：

    KEAM模型(EDM+ICCE) limitation: 没层级关系。可以拓展这个地方在model design。不一视同仁看各个entity。
    CELDA limitation: 只生成了concept信息，并且评估方法比较一般。

可以后续生成relation信息(concept-concept, course-course)， 再用于KEAM的结构。Relation：prerequisite-dependence, similarity.




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


## MOOCCube & MOOCCubeX





