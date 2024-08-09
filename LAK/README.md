
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
    # course (need) taxonomy (prerequisite)
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
    # (taxonomy (similar) taxonomy)
    

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


图示：

    Student - LearnConcept - Concept - IncludedBy - Course
    Student - TaughtBy - Teacher - Teach - Course
    Student - StudyAt - School - Offer - Course
    Student - LearnConcept - Concept - IncludedBy - Taxonomy - IncludedBy - Course
    Student - LearnConcept - Concept - SimilarConcept - Concept - IncludedBy - Course
    Student - LearnConcept - Concept - PrerequisiteDependency - Concept - IncludedBy - Course


没有讨论的是 课程间的 先后关系， 侧重于其中的concept 和 taxonomy的先后关系。




## Result

Performance comparison(Top10):

    KEAM: {'Precision': 0.0598247535596989, 'Recall': 0.42288428697382024, 'HR': 0.5108433734939759, 'NDCG': 0.3211177144031813}
    
    BEST: (OrderedDict([('recall@10', 63.1065), ('precision@10', 7.1953), ('ndcg@10', 51.7822), ('hit@10', 71.8926)])
    w/o kge: (OrderedDict([('recall@10', 58.0343), ('precision@10', 6.8221), ('ndcg@10', 48.3729), ('hit@10', 68.2752)])
    w/o DSMM: OrderedDict([('recall@10', 60.197), ('precision@10', 7.1533), ('ndcg@10', 49.5681), ('hit@10', 70.8223)])
    w/o AMA: OrderedDict([('recall@10', 57.7709), ('precision@10', 6.5461), ('ndcg@10', 33.7881), ('hit@10', 66.4122)])
             
    w/o similar course: OrderedDict([('For student', OrderedDict([('recall@10', 0.7713), ('precision@10', 0.0771), ('ndcg@10', 0.5202), ('mrr@10', 0.4407), ('hit@10', 0.7714)]))])
    w/o teacher:  OrderedDict([('recall@10', 0.5908), ('precision@10', 0.0591), ('ndcg@10', 0.4121), ('mrr@10', 0.356), ('hit@10', 0.5909)])
    w/o school: OrderedDict([('recall@10', 0.6552), ('precision@10', 0.0655), ('ndcg@10', 0.4286), ('mrr@10', 0.358), ('hit@10', 0.6553)])
    w/o similar concept: OrderedDict([('recall@10', 0.679), ('precision@10', 0.0679), ('ndcg@10', 0.4424), ('mrr@10', 0.3684), ('hit@10', 0.6791)])
    w/o pre-dep concept: OrderedDict([('recall@10', 0.6673), ('precision@10', 0.0667), ('ndcg@10', 0.4411), ('mrr@10', 0.3704), ('hit@10', 0.6674)])
    w/o taxonomy: OrderedDict([('recall@10', 0.77), ('precision@10', 0.077), ('ndcg@10', 0.4183), ('mrr@10', 0.3108), ('hit@10', 0.7701)])
    w/o concept: OrderedDict([('recall@10', 0.9448), ('precision@10', 0.0945), ('ndcg@10', 0.5712), ('mrr@10', 0.453), ('hit@10', 0.9449)])
    w/o hierarchical: OrderedDict([('recall@10', 0.7704), ('precision@10', 0.077), ('ndcg@10', 0.4851), ('mrr@10', 0.3956), ('hit@10', 0.7704)])
    ------------------------------------
    MAECR: OrderedDict([('recall@10', 63.1065), ('precision@10', 7.1953), ('ndcg@10', 51.7822), ('hit@10', 71.8926)])
    MAECR w/o taxonomy: OrderedDict([('recall@10',58.9289), ('precision@10', 6.9725), ('ndcg@10', 41.8913), ('hit@10', 69.3281)])
    MAECR w/o concept: OrderedDict([('recall@10', 53.0089), ('precision@10', 6.0945), ('ndcg@10', 40.6962), ('hit@10', 62.3634)])
    MAECR w/o teacher:  OrderedDict([('recall@10', 50.2306), ('precision@10', 5.9521), ('ndcg@10', 41.1421), ('hit@10', 59.0948)])
    MAECR w/o school: OrderedDict([('recall@10', 55.7059), ('precision@10', 6.5519), ('ndcg@10', 42.4686), ('hit@10', 65.5363)])
    
    MAECR w/o similar course: OrderedDict([('recall@10', 59.0204), ('precision@10', 7.0154), ('ndcg@10', 50.0215), ('hit@10', 69.4358)])
    MAECR w/o similar concept: OrderedDict([('recall@10', 57.7250), ('precision@10', 6.7928), ('ndcg@10', 44.2494), ('hit@10', 67.9118)])
    MAECR w/o pre-dep concept: OrderedDict([('recall@10', 56.7368), ('precision@10', 6.6723), ('ndcg@10', 44.1108), ('hit@10', 66.7492)])
    MAECR w/o hierarchical: OrderedDict([('recall@10', 57.9136), ('precision@10', 6.9012), ('ndcg@10', 48.5187), ('hit@10', 68.1336)])














# GPT

我现在想将用自己的数据套用在kaerr上，所以需要把数据处理成他的样子，他的最原始数据为3个.txt文件 user.txt, job.txt, inter.txt，分别存了用户信息，工作信息，交互信息。
存储格式为：
user_id	live_city_id	desire_jd_city_id	desire_jd_industry_id	desire_jd_type_id	desire_jd_salary_id	cur_industry_id	cur_jd_type	cur_salary_id	cur_degree_id	birthday	start_work_date	experience
17e1b9f107dd1214bd78dec6d91593a4	551	551,-,-	房地产/建筑/建材/工程	工程造价/预结算	0100002000	房地产/建筑/建材/工程	土木/建筑/装修/市政工程	0200104000	大专	24	2017	停车|现场|凤凰|预算编制|建设|实习|专家|公园|预算软件|勘察|合同|知识|商务|单位|计量软件|隧道|协调|投标|商业|标书制作|住宅|技术|结算|桥梁|学习|土建|设计|标书|市场营销部|服务|进度|门窗|景观|推进|收款|资料|造价员|报告|运动|工程|工程项目|招投标|沟通|收集|管道|工业|可行性研究|铝合金|计量|市场营销|工程设计|业主|制作|预算员|道路|计价软件|编制|市政道路|市场|外联|建筑设计|市政|软件|车库|污水
0c02d9411e83ae0308cdc40700385d4c	763	763,-,-	其他	化妆师	0400106000	房地产/建筑/建材/工程	后期制作	0400106000	大专	24	2015	调色员|彩妆|护肤|布料|光源|客户|调色


我的数据是好多个.json文件，所以我首先需要将他们合并成3个.txt 或者3个.csv文件用于后续处理。你理解我的要求了吗，在你理解后，我会详细介绍我的需求。

我首先介绍我想要的3个文件里都包含哪些信息：
user文件：user_id, user_name, learned_concept, learned_taxonomy
course文件: course_id, course_name, c_concept, c_taxonomy, teacher, school, pre_concept
interaction文件: user_id course_id enroll
我解释一下上述信息：
用户文件中 id name 学习过的concept 学习过的taxonomy 根据交互的课程的concept和taxonomy来确定。
course文件 id name concept taxonomy teacher school 根据文件直接获取，pre_concept, pre_taxonomy 根据概念间的先修关系等确定，后面会详细介绍。
interaction文件 的交互信息可以直接从json文件获取，第三列的enroll全为1.

然后我介绍一下这些信息从哪些文件(我现在的数据)获取：
我有user.json 包含了id 和name字段 格式如下：{"id": "U_7001215", "name": "李喜锋", "course_order": ["C_course-v1:TsinghuaX+00740043_2x_2015_T2+sp", "C_course-v1:TsinghuaX+30240184+sp", "C_course-v1:TsinghuaX+00740043X_2015_T2+sp", "C_course-v1:TsinghuaX+10421094X_2015_2+sp", "C_course-v1:TsinghuaX+30240184_2X+sp"], "enroll_time": ["2017-05-01 11:07:53", "2017-05-17 10:07:17", "2017-05-01 11:09:21", "2017-11-30 14:14:40", "2017-08-18 21:21:32"]}

我有course.json 包含了id 和name 字段，保存格式如上为字典。
我有concept.json文件 包含了concept信息和taxonomy信息 分别为：
{"id": "K_《三国史记》_世界历史", "name": "《三国史记》", "en": "History of Three States in Ancient Korea", "explanation": "学科：世界历史_古代中世纪_东北亚 定义：朝鲜现存的最古史书，1145年高丽王朝的学者金富轼(1075-1151)用古汉语撰成，记述了新罗、高句丽和百济三国的史事。 见载：《世界历史名词》第一版"}
{"id": "K_T_世界历史_世界历史", "name": "世界历史"}
我们只需要他的id（K_xx, K_T_xxx）保存到c_concept, c_taxonomy。

我们有user-course.json 文件 只有2列 为user_id course_id,格式为：U_7001215	C_course-v1:TsinghuaX+00740043_2x_2015_T2+sp
U_7001215	C_course-v1:TsinghuaX+30240184+sp

我们有course-concept文件 也是2列（课程id和concept）：C_course-v1:KMUSTX+1803168+2019_T1	K_活性炭_化学
C_course-v1:KMUSTX+1803168+2019_T1	K_内切_数学

我们也有concept-field文件 也是2列(concept和taxonomy)，表明了concept隶属于哪个taxonomy：K_《三国史记》_世界历史	K_T_世界历史_世界历史
K_T_世界历史_世界历史	K_T_世界历史_世界历史

我们利用上面的3个json文件就可以，制作出interaction交互文件，和对应的c_concept,c_taxonomy,learned_concept,learned_taxonomy列。

我们还有teacher-course.json 和school-course.json 用于制作第课程数据文件的teacher school列，格式为：
T_方维奇	C_course-v1:SPI+20170828001x+sp
T_方维奇	C_course-v1:SXPI+20170828001x+2019_T1

S_BNU	C_course-v1:BNU+CSL21148501+2018_T2
S_BNU	C_course-v1:BNU+GE310141091+2019_T1

最麻烦的来了！我们在course文件中设计了一个pre_concept，想保存学习该课程前你需要学习哪些concept和taxonomy，我们有一个prerequisite-dependency.json文件，保存了两个concept的先后关系：
K_计算机科学_计算机科学技术	K_服务器_计算机科学技术
K_服务数据单元_计算机科学技术	K_缓存_计算机科学技术
根据每个课程所拥有的concept，我们要整理出他所需要的concept存起来。




