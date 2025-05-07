# LLM-based CRS工作思路


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
| relation生成 |  | 给Pair concepts 输出relation(做相关) |
| relation评估 |  | 1 |
| Metircs | 相似分数？ + F1, Recall, Precision | F1, Recall, Precision |


可深化内容：

    1. 多选concept生成中，可以有个根据置信度生成。这样就可以topK了(AIED没做，journal补充一下)
    2. 多选concept生成中，可以指定列表长度(10+10)，或者进行0-1之间预测，找个阈值。

开放式concept生成！
LLMs model：

    Finished： gpt-3.5-turbo, gpt-4-turbo, gpt-4o(gpt-4o-2024-08-06), gpt-4o-mini, o1-mini
    Doing： gpt-4-0613


|            | Text | w/oText | Concept | Description | Name | ZeroShot |
|------------|------------|------------|------------|------------|------------|------------|
| gpt-3.5-turbo | √ | √ |√ |√ |√ |√ |
| gpt-4-turbo | √ | √ |√ |√ |√ |√ |
| gpt-4o(gpt-4o-2024-08-06) | √ | √ | √ | √ | √ | √ |
| gpt-4o-mini | √ | √ | √ | √ | √ | √ |
| o1-mini | √ | √ | √ | √ | √ | √ |

开放式relation生成！
LLMs model：

    Finished： 
    Doing： gpt-3.5-turbo, gpt-4-turbo, gpt-4-0613, gpt-4o(gpt-4o-2024-08-06), gpt-4o-mini, o1, o1-mini

多选任务concept生成！
LLMs model：

    Finished： gpt-3.5-turbo, gpt-4o(gpt-4o-2024-08-06), gpt-4o-mini
    Doing： gpt-4-turbo, gpt-4-0613, o1-mini

1.能不能解决语义飘逸 (或者哪个模型处理这个更好？怎么评估语义飘逸)
2.设计错误选项的概念列表。 1+1 同或者不同field课程 (1+2)
    
多选relation生成！
LLMs model：

    Finished： 
    Doing： gpt-3.5-turbo, gpt-4-turbo, gpt-4-0613, gpt-4o(gpt-4o-2024-08-06), gpt-4o-mini, o1, o1-mini






## Large Language Models for Recommendation: Progresses and Future Direction (AIED后边的工作思路)

还有PPT

[https://www.youtube.com/watch?v=zcuOrWxJ2k8](https://www.youtube.com/watch?v=zcuOrWxJ2k8)


## 笔记

LLMs 推荐系统 [笔记](https://f0jb1v8xcai.feishu.cn/wiki/DMqOwfhbIi2kUlkxu0RcVaYjn2F)


## Is ChatGPT a Good Recommender? A Preliminary Study


## Evaluating ChatGPT as a Recommender System: A Rigorous Approach


Try OpenP5- https://yongfeng.me/attach/LLM4RecSys.pdf

    
    
## MOOCCube & MOOCCubeX





