
# Why bi-directional model student and course?

1. 捕捉双重需求
在学生和课程推荐系统中，双向视角可以更全面地捕捉双方的需求和特点：

学生视角
个性化需求：每个学生都有独特的兴趣、知识背景和学习目标。单纯从课程视角考虑，可能忽略了学生的个性化需求。
学习历史和行为：学生的学习历史和行为模式（如已选修课程、表现和兴趣领域）对推荐有重要影响。
课程视角
课程特点：课程有特定的内容、知识点和目标受众。不同课程对学生的知识基础和兴趣有不同要求。
匹配度：从课程的视角分析，有助于评估课程是否适合学生的背景和需求。

2. 提高推荐准确性
通过双向建模，可以综合考虑学生和课程的双重需求，提高推荐的精准度：

学生匹配度：从学生的兴趣和学习背景出发，找到最合适的课程。
课程匹配度：从课程的内容和要求出发，找到最适合的学生。

3. 增强推荐的解释性
双向视角提供了更好的解释性，可以回答“为什么推荐这个课程给这个学生”以及“为什么这个学生适合这个课程”：

学生视角的解释：推荐某课程给某学生，可以解释该课程如何符合学生的兴趣和学习需求。
课程视角的解释：推荐某学生选修某课程，可以解释该学生的背景和兴趣如何符合课程的要求。

4. 案例对比
用一个具体的例子来说明双向建模的优势：

单向视角（仅学生视角）
学生A：对机器学习感兴趣，推荐“机器学习基础”课程。
问题：如果“机器学习基础”课程需要高级编程技巧，而学生A没有相应的编程基础，这个推荐可能并不合适。

双向视角（学生和课程双向）
学生A：对机器学习感兴趣，且有基础编程技能。
课程“机器学习基础”：需要学生有基础编程技能。

结果：基于双向视角，系统能够推荐“机器学习基础”课程给有相应编程基础的学生A，确保推荐的合理性和成功率。

总结
采用双向建模有助于综合考虑学生和课程双方的需求，提供更加精准和个性化的推荐。同时，这种方法增强了推荐系统的解释性，使得推荐结果更加透明和可信。在MOOCCube数据集的背景下，尽管关系类型较少，通过优化元路径设计和注意力机制，仍可以实现高效的双向建模。

# Goal-based

motivation:

    1.多学科学术兴趣的增加：学生对多个不同学科领域的兴趣和探索意愿。随着现代教育的发展，学生们越来越倾向于跨越传统学科界限，去探索和学习多种学科的知识。例如，一名学生可能主修计算机科学，但同时对心理学或艺术历史表现出浓厚的兴趣，并希望在这些领域选修课程。然而先修课程信息不太行：先决条件可能不够更新；缺乏综合性，忽略了不同部门的课程组合；不考虑学生已经掌握的知识，学生常常会绕过这些先决条件；课程人数满了 换课程
    2.学生决策的复杂性：学生需要在不同的专业、探索的主题和课程负荷的难度之间做出平衡。这些决策涉及风险与回报的权衡，而学生希望最大化多个目标并规避风险（例如，选择对雇主有价值的挑战性课程，同时保持高GPA）。
    3.学术咨询资源的限制：现有的学术咨询资源如教师和非教师学术顾问都有各自的局限性。教师通常是特定领域的专家，而非教师学术顾问则具备广泛的课程熟悉度，但在深度上有所不足。
    4.个性化推荐的必要性：学生的个性化需求和学习背景各不相同，单纯的基于课程的推荐可能不足以满足学生的需求。

最近发展区（Zone of Proximal Development, ZPD）：指学生在有适当帮助时可以完成的学习任务。简单来说，就是学生虽然自己做不了，但在帮助下可以学会的内容

实现：LSTM 各种魔改输入

# A Prerequisite Attention Model for Knowledge ProficiencyDiagnosis of Students (CIKM2022)

提出一个模型通过学习知识概念间的前置关系，尤其是不同前置概念对后续概念的差异化影响，来提高诊断性能。通过学生的响应记录和知识前置关系图，能够有效推断学生的知识熟练度。

input: 学生 练习 知识概念 先决条件的一个矩阵

该论文模拟 练习的 难度和区分度的方法解释不清，但可以仿照去模拟。


# RCD: Relation Map Driven Cognitive Diagnosis for Intelligent Education Systems

一个GNN技术 来聚合 student exercise concept。 concept dependency 有2种 prerequisite 和 similarity （可用·），聚合 concept时候 考虑的concept k 的关系为k的相似的，和k的前驱。(再用注意力决定各部分占比)


# Process

## KAERR 

user_file, job_file, inter_file.

KG:

Node: user job city industry salary job_type degree year experience

## Raw data

student_file1, course_file2, enrollment_file3

## KG

Node(Entity): 

student(user), course(item), school(开设课程的学校), teacher(属于哪个学校), concept(K_), taxonomy(K_T_). (experience? similar)

Relation(Edge):

    student (enrolled) course
    student (上过该校的course) school
    student (上过该老师的course) teacher
    student (learned) concept
    student (learned) taxonomy
    (student (similar) student)
    course (is enrolled by) student
    course (被开设 by) school
    course (被担任 by) teacher
    course (涉及) concept
    course (涉及) taxonomy
    course (need) concept (prerequisite)
    course (need) taxonomy (prerequisite)
    (course (similar) course)
    school (开设) course
    # school (has) teacher
    # teacher (任职于) school
    teacher (负责) course
    concept (is learned by) student
    concept (is included by) course
    concept (属于) taxonomy
    concept (need) concept (prerequisite)
    (concept (similar) concept)
    taxonomy (is learned by) student
    taxonomy (is included by) course
    taxonomy (包含) concept
    taxonomy (包含) taxonomy (parent-son)
    taxonomy (need) taxonomy (prerequisite)(可能有)
    (taxonomy (similar) taxonomy)
    

Assitant:

实体和关系定义
实体
学生（user）：包括学生的基本信息和学习历史。
课程（course）：包括课程的基本信息、涉及的知识点（concept）、相关领域（field）、学校（school）和教师（teacher）。
知识点（concept）：课程中涉及的具体知识点。
领域（field）：课程所属的领域。
学校（school）：提供课程的学校。
教师（teacher）：教授课程的教师。
关系
学生-课程（user-course）：学生与课程之间的交互关系（选修或完成）。
课程-知识点（course-concept）：课程中涉及的知识点。
课程-领域（course-field）：课程所涉及的领域。
课程-学校（school-course）：课程所属的学校。
课程-教师（teacher-course）：课程的教授教师。

## Meta-path

student - relation1 - entity1 - relation2 - entity2 - reverse_relation1 - course

Detailed design:

    student - taught by - teacher - teaches - course
    student - study at - school - offers - course
    student - learn - concept - included in - course
    student - learn - taxonomy - included in - course
    student - learn - concept_prereqiusite - pre_dep - concept_dependency - course (关系的前后需求)
    student - learn - concept - required by - course (concept 被课程需要)
    student - learn - taxonomy - required by - course
    student - enroll in - course A - similar to - course B
    student A - similar to - student B - enroll in - course
    student - learn - concept A - similar to - concept B - course
    student - learn - taxonomy A - similar to - taxonomy B - course
    student - learn - concept - included in - taxonomy - course
    student - learn - taxonomy - includes - concept - course
    student - learn - taxonomy - son_parent - taxonomy - course


没有讨论的是 课程间的 先后关系， 侧重于其中的concept 和 taxonomy的先后关系。









