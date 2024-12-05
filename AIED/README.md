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



## Course Concept Expansion in MOOCs with External Knowledge and Interactive Game






## Course Concept Extraction in MOOCs via Embedding-Based Graph Propagation





## Prerequisite Relation Learning for Concepts in MOOCs 

提出一个机器学习方法 预测MOOCs 中知识概念之间的先修关系，利用**语义信息，上下文特征，结构特征**3个方面的信息，进行综合建模。再找各种分类器做实验。

涉及的feature：

    1. Semantic Relatedness
    2. Video Reference Distance, Sentence Reference Distance, Wikipedia Reference Distance
    3. Average Position Distance, Distributional Asymmetry Distance, Complexity Level Distance

方法有点老，且不好复现。但提供了一个小数据，可以用来后续的**comparison**. (GPT生成一份，看看差距)

验证过程：人工标签后，把这些概念embedding 送进分类器预测。(不是很好去拿过来用。没法这么弄GPT生成的概念)


## MOOCCube & MOOCCubeX





