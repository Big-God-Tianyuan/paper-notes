## MAKE工作延展

    1. 学生评价概念 + xxx?

    2. 设计一个模型 可以把生成的概念用于推荐系统 + explanation

    3. 用生成的概念去做解释推荐结果等 （用户画像生成，学习目标总结，goal驱动的推荐系统，可解释的推荐系统等）

## BDCC工作延展

    1. 增加解释模块：把解释模块与LLM 生成概念结合。 EDM2025 目前仅使用课程描述（content-based 或 outcome-based）生成解释。在生成解释时，把“课程概念列表”（GPT 生成的）作为结构化输入(e.g.)：explanation 自动生成 “你过去擅长的 Optimization 与本课程中的 Regularization 概念高度相关，因此推荐 X。”
    
    2.

## 后续工作方向

    1. LLM判断方向，关系

    2. 

## EDM/ICCE工作延展

    1. User Study. 设计交互界面，探究可解释性。大问卷，不同的profile，不同的推荐呈现形式的各种指标。多维度评估推荐结果。
    
    2. LLM介入. 利用LLM优化推荐 代替部分推荐模块

    3. LLM生成profile 与EDM的profile比较。需要用户。

    4. 改善autoencoder结构，并引入GPT生成的概念。

    5. 把 KEAM 的知识画像作为 解释模块输入，让 LLM 用这些显式数据构造更可信的解释 (结合EDM2025: Content-based + Relevance Theory + Cognitive Verifier = 最有效解释（模型 D） + 解释要“建立过去课程与推荐课程之间的明确关联”)  , 最终得到：KEAM-XAI，一个可解释模块 + 显式知识实体画像 + 内容描述。


## LAK工作延展

    1. Meta-path 这个点很重要，用户实验可以来证明解释性。(后续，暂时不考虑)
    
    2. ...
