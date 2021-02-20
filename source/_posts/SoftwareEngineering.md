---
title: Software Engineering Review
date: 2019-01-13 13:16:18
tags:
	-  software engineering 
	- review
categories:
	- review
---

# Chapter 00 Intorduction
* 软件工程的概念、方法和技术：
  * 软件工程基本概念（软件产品、软件过程、软件开发模型）
  * 软件工程开发方法和技术
    * 传统的软件工程方法与技术
    * 面向对象的软件工程方法与技术
  * 软件测试策略和技术
  * 软件项目管理
# Chapter 01 Software and Software Engineering
* Software's dual role
  * Software is a **product** (软件即产品（服务）)
    * Transforms information - produces, manages, acquires, modifies, displays or transmits information;
    * Delivers computing potential of hardwora and networks.
  * Software is a **vehicle(载体)** for delivering a product.
* Hardware & Software
* wear out vs. Deterioration 软件老化和变质
* ![1547359380593](/Review/SoftwareEngineering/1547359380593.png)

*   Component Based & Custom Built

    *   The software industry dose seem to be moving (slowly) toward component-based construction.\
*   Software Complexity

    *   <font color = red>**No Silver Bullet** </font>(智力密集型没有最优解不可预估bug不可避免)
*   >   Software changeability 软件可更改性
    >   *   It must be fixed to eliminate errors. 必须对其进行修复以消除错误。
    >   *   It must be enhanced to implement new functional and non-functional requirements 必须对其进行改进, 以实现新的功能和非功能要求
    >   *   Software must be adapted to meet the needs of new computing environments or technology.软件必须进行调整, 以满足新的计算环境或技术的需要。
    >   *   Software must be enhanced to implement new business requirements.必须增强软件以实现新的业务需求。
    >   *   Software must be extended to make it interoperable with other more modern systems or databases.软件必须进行扩展, 以使其可与其他更现代的系统或数据库进行互操作。
    >   *   Software must be re-architected to make it viable(切实可行的) within a network environment.必须重新构建软件, 使其在网络环境中可行。
*   >   Software Evolution[Lehman定律]（记标题就行）
    >   *   The Law of Continuing Change [持续变化规律] (1974):  E-type systems must be continually adapted else they become progressively less satisfactory.
    >   *   The Law of Increasing Complexity [复杂性增长规律] (1974):  As an E-type system evolves its complexity increases unless work is done to maintain or reduce it.
    >   *   The Law of Self Regulation [自我调控规律] (1974):  The E-type system evolution process is self-regulating with distribution of product and process measures close to normal.
    >   *   The Law of Conservation of Organizational Stability [组织稳定性守恒规律] (1980):  The average effective global activity rate in an evolving E-type system is invariant over product lifetime.
    >   *   The Law of Conservation of Familiarity [保证通晓性规律] (1980): As an E-type system evolves all associated with it, developers, sales personnel, users, for example, must maintain mastery of(熟悉) its content and behavior to achieve satisfactory evolution.
    >   *   The Law of Continuing Growth [持续增长规律] (1980):  The functional content of E-type systems must be continually increased to maintain user satisfaction over their lifetime.
    >   *   The Law of Declining Quality [质量衰减规律] (1996): The quality of E-type systems will appear to be declining unless they are rigorously maintained and adapted to operational environment changes.
    >   *   The Feedback System Law [反馈系统规律] (1996):  E-type evolution processes constitute multi-level, multi-loop, multi-agent feedback systems and must be treated as such to achieve significant improvement over any reasonable base.
*   Software **Myths** 软件谬论
    *   Software Myths affect managers, customers (and other non-technical stakeholders) and practitioners

    *   Software Myths are believable because they often have elements of truth,
        *   but …
            *   Invariably lead to bad decisions,
        *   therefore …
            *   Insist on reality as you navigate your way through software engineering

    *   If we get behind schedule, we can **add more programmers** and catch up.

    *   **A general statement about objectives** is sufficient to begin building programs.

    *   Change in project requirements can be **easily accommodated** because software is flexible.

    *   **Management Myths**

>   “We already have a book of standards and procedures for building software. It does provide my people with **everything** they need to know …”
>
>   “If my project is behind the schedule, I always can add more programmers to it and catch up …”
>
>   “If I decide to **outsource** the software project to a third party, I can just relax: Let them build it, and I will just pocket my profits …”

*   **Customer Myths**

>   “A general statement of objectives is sufficient to begin writing programs - we can fill in the details later …”
>
>   “Project requirements continually change but this change can easily be accommodated because software is flexible …”

*   **Practitioner’s Myths**

>“Let’s start coding ASAP, because once we write the program and get it to work, our job is done …”
>
>“Until I get the program running, I have no way of assessing its quality …”
>
>“The only deliverable work product for a successful project is the working program …”
>
>“Software engineering is baloney[胡扯]. It makes us create tons of paperwork, only to slow us down …”

# Chapter 02 Process 软件过程（综述）

*   Overview

*   What? 过程是什么？当开发产品或构件系统时，遵循一系列可预测的步骤（即路线图）是非常重要的，它有助于及时交付高质量的产品。

*   Who? 相关人员？管理人员、软件工程师和客户均应该参与过程的定义、建立和测试。

*   Why?重要性？提高了软件开发活动的稳定性、可控性和有组织性；否则软件活动会失控并变得混乱。

*   Steps?有哪些步骤？ 具体步骤随着所构造的软件类型不同在细节方面有所变化，但对所有过程来讲有很多活动是相同的。

*   Work product?有哪些工作产品？ 是指过程中定义的一系列活动和任务的结果，包括Programs, documents, and data.

*   Correct process?什么是正确的过程？ Assessment, quality deliverable.

*   IEEE Definition

    >Software Engineering: (1) The application of a systematic, disciplined, quantifiable[系统的、规范的和可量化的] approach to the development, operation, and maintenance of software; that is, the application of engineering to software. (2) The study of approaches as in (1).

![1547363766241](/Review/SoftwareEngineering/1547363766241.png)

*   软件过程

    *   软件过程是一个为建造高质量软件所需完成的任务的框架，即形成软件产品的一系列步骤。包括中间产品、资源、角色及过程中采取的方法、工具等范畴。

*   Software process model

    *   Attempt to organize the software life cycle by
        *   defining activities involved in software production[软件生产]
        *   defining order of activities and their relationships
    *   Goals of a software process
        *   standardization, predictability, productivity, high product quality, ability to plan time and budget requirements

*   早期做法： Code & Fix

    	>The earliest approach
    	>
    	>*   Write code
    	>
    	>*   Fix it (修复) to eliminate any errors that have been detected, to enhance existing functionality, or to add new features
    	>
    	>*   Source of difficulties and deficiencies
    	>    *   impossible to predict（不可预测性）
    	>    *   impossible to manage

*   软件危机 Symptoms of inadequacy: **the software crisis**

    *   **scheduled time and cost exceeded**

    *   **user expectations not met**

    *   **poor quality**

    The **size** and **economic value** of software applications require appropriate "**process models**"

*   Process model goals (B.Boehm 1988)

>   *   determine the order of stages involved in software development and evolution, and to establish the **transition criteria（标准尺度）** for progressing from one stage to the next.  These include completion criteria for the current stage plus choice criteria and entrance criteria for the next stage. Thus a process model addresses the following software project questions:确定软件开发和进化所涉及的阶段的顺序, 并建立从一个阶段到下一个阶段的过渡标准。 其中包括当前阶段的完成标准加上选择标准和下一阶段的标准。因此, 流程模型解决了以下软件项目问题:
>
>      *   What shall we do next?
>
>      *   How long shall we continue to do it?

*   软件过程

    *   黑盒观点

    *   ![1547365114325](/Review/SoftwareEngineering/1547365114325.png)

        *   Problems

            *   The assumption is that requirements **can be fully understood** prior to development

            *   Interaction with the customer occurs **only** at the beginning (requirements) and end (after delivery)

            *   Unfortunately the assumption almost **never holds**

    *   白盒观点

    *   ![1547365404494](/Review/SoftwareEngineering/1547365404494.png)

        *   Advantages
            *   Reduce risks by improving visibility
            *   Allow project changes as the project progresses
                *   based on feedback from the customer

    *   软件开发活动

        *   线性过程模型
        *   非线性模型

*   过程框架

    ![1547365505796](/Review/SoftwareEngineering/1547365505796.png)

*   <font color = red>**通用活动框架（非常重要）**</font>

    ![1547365900978](/Review/SoftwareEngineering/1547365900978.png)![img](/Review/SoftwareEngineering/1547367400287.png)![1547367504297](/Review/SoftwareEngineering/1547367504297.png)

    ![img](/Review/SoftwareEngineering/1039166-20170321200512502-745718093-1547367531435.png)

*   普适性活动 Umbrella Activities
    *   Software project management
    *   Formal technical reviews
    *   Software quality assurance
    *   Software configuration management
    *   Work product preparation and production
    *   Reusability management
    *   Measurement
    *   Risk management
*   能力成熟度模型集成**(CMMI)
*   过程评估 Process Assessment
    *   评估软件过程以确认满足了成功软件工程所必需的基本过程标准(basic
        process criteria**)**要求.The process should be assessed to ensure that it meets a set of basic process criteria that have been shown to be essential for a successful software engineering.
    *   ![1547367730629](/Review/SoftwareEngineering/1547367730629.png)

# Chapter 03 Process Models

*   软件生命周期、概念、阶段
*   软件过程模型

![1547451466048](/Review/SoftwareEngineering/1547451466048.png)

*   Prescriptive Models[惯例模型]

    *   Prescriptive process models advocate an orderly approach to software engineering.

        *   question
            *   If prescriptive process models strive for structure and order, are they inappropriate for a software world that thrives on change? 
            *   Yet, if we reject traditional process models (and the order they imply) and replace them with something less structured, do we make it impossible to achieve coordination and coherence in software work?

    *   The waterfall Model 瀑布模型

    *   ![1547451662260](/Review/SoftwareEngineering/1547451662260.png)

    *   >   (适用于需求较为固定和明确的场景、不适应大量、频繁的更改)
        >   1.  The requirements are **knowable** in advance of implementation.
        >   2.  The requirements have **no unresolved, high-risk implications.**
        >   * risks due to COTS choices, cost, schedule, performance, safety, security, user interfaces, organizational impacts.
        >   3.  The nature of  the requirements will **not change very much.** 
        >   *   During development; during evolution.
        >
        >   4.  The requirements are **compatible with** all the key system stakeholders’ expectations. 
        >       *   e.g., users, customer, developers, maintainers, investors.
        >   5.  The **right architecture** for implementing the requirements is well understood.
        >   6.  There is **enough calendar time** to proceed sequentially.~

    *   The V Model V模型

        *   ![1547454083297](/Review/SoftwareEngineering/1547454083297.png)

    *   Incremental Models 增量模型

    *   *   ![1547454772132](/Review/SoftwareEngineering/1547454772132.png)
        *   RAD 模型
        *   ![1547454814108](/Review/SoftwareEngineering/1547454814108.png)

    *   Evolutionary Models 演化模型

       *   >   客户不确定要求 工程师对算法效率 可用性不确定
           >
           >   帮助客户和工程师了解要构建的内容快速设计和实现

       *   Prototyping 原型

       *   ![1547455509874](/Review/SoftwareEngineering/1547455509874.png)

       *   >原型范式中的问题
           >sw 工程师尝试修改原型以用作工作版本
           >一旦客户看到工作原型, 她希望很快就能得到工作产品
           >

       * The Spiral 螺旋形

           *   ![1547455726118](/Review/SoftwareEngineering/1547455726118.png)

       * Full Spiral Model 

           *   Radial dimension[按射线方向]: cumulative cost to date
           *   Angular dimension[按螺旋方向]: progress through the spiral
           *   ![1547455837090](/Review/SoftwareEngineering/1547455837090.png)

       *   UP Unified Process Model 统一过程模型

           *   用例驱动
           *   以体系结构为中心
           *   迭代和增量
           *   jia'gou
           *   ![1547455977324](/Review/SoftwareEngineering/1547455977324.png)
           *   Life cycle![1547455998301](/Review/SoftwareEngineering/1547455998301.png)

# Chapter 04 Agile Development

*   敏捷开发

    *   适应变更
    *   交流通畅
    *   客户参与
    *   有效控制
    *   原则
        *   尽早交付、持续交付
        *   欢迎变更、创造优势
        *   经常交付、间隔紧凑
        *   开发期间业务人员和开发人员在一起工作
        *   围绕受激励的个人、提供环境和支持并信任
        *   团队内部面对面交谈最有效率效果
        *   可持续开发、赞助人开发者用户长期稳定的开发速度
        *   关注优秀技能提升敏捷能力
        *   保持简单
        *   好的架构、需求、设计出自组织团队
        *   定时反省、及时调整
    *   ![1547470424903](/Review/SoftwareEngineering/1547470424903.png)

    >   **Individuals and interactions**(交互) over processes and tools 
    >
    >   **Working software** over comprehensive documentation 
    >
    >   **Customer collaboration** over contract negotiation (谈判)
    >
    >   **Responding to change** over following a plan
    >
    >   ​	(No sliver bullet 自相矛盾而无可奈何)
    >
    >   增量发布\多生命周期
    >
    >   交付产品
    >
    >   *   可运行软件
    >   *   源代码
    >       *   使用权
    >       *   销售权

*   极限编程 XP

*   Begins with the creation of user stories

*   Agile team assesses each story and assigns a cost

*   Stories are grouped to for a deliverable increment

*   A commitment[承担义务] is made on delivery date[交货日期]

*   After the first increment, project velocity is used to help define subsequent delivery dates for other                                                increments

*   ![1547471247252](/Review/SoftwareEngineering/1547471247252.png)

*   ![1547472622915](/Review/SoftwareEngineering/1547472622915.png)

*   ![1547473560970](/Review/SoftwareEngineering/1547473560970.png)

# Chapter 06 系统工程

*   <font color = red>系统工程中的概念</font>
    *   元素    （软件生态系统）
        *   软件
        *   硬件
        *   **人**
        *   数据库（数据）
        *   文档
        *   规程
    *   层次体系
        *   ![1547474492979](/Review/SoftwareEngineering/1547474495591.png)
*   系统建模
    *   定义在所考虑视图中满足需要的过程
    *   描述过程行为和该行为所依据的假设
    *   明确定义模型的外在和内在输入
    *   描述有助于工程师理解视图的全部联系
*   系统建模分类
    *   Business Process Engineering(PBE)

# Chapter 07 需求工程

*   需求工程任务

    *   初启Inception—Establish a basic understanding of the problem and the nature of the solution.

    *   启发Elicitation—Draw out the requirements from stakeholders.

    *   细化Elaboration—Create an analysis model that represents information, functional, and behavioral

         aspects of the requirements.

    *   协商Negotiation—Agree on a deliverable system that is realistic for developers and customers.

    *   规范Specification—Describe the requirements formally or informally.

    *   验证Validation—Review the requirement specification for errors, ambiguities, omissions, and conflicts. 

    *   需求管理Requirements management—Manage changing requirements.

*   需求工程工作产品

    *   可行性和必要描述
    *   系统或产品的范围说明
    *   参与需求导出的客户、用户和其他利益共同者的列表
    *   系统技术环境的说明
    *   需求列表（含领域限制）
    *   一系列使用场景
    *   任何能更好的定义需求的原型

*   <font color = red>需求开发方法</font>

    *   需求获取：1 开始过程：与客户建立初步交流。2 导出过程：通过访问和调查，获得需求的描述 
    *   需求分析：精化过程，通过分析建模，建立精确的技术模型，说明软件功能，特征和约束。 
    *   需求处理：1 协商过程 2 形成规格说明 3 需求确认

# Chapter 08 模型分析

*   分析模型的作用
    *   specifies software’s operational characteristics
    *   indicates software's interface with other system elements 
    *   establishes constraints that software must meet
*   分析模型的构建原则（经验原则）
    *   The model should focus on requirements that are visible within the problem or business domain. The level of abstraction should be relatively high. 
    *   Each element of the analysis model should add to an overall understanding of software requirements and provide insight into the information domain, function and behavior of the system.
    *   Delay consideration of infrastructure and other non-functional models until design. 
    *   Minimize coupling throughout the system. 
    *   Be certain that the analysis model provides value to all stakeholders. 
    *   Keep the model as simple as it can be. 
*   <font color = red>方法</font>
    *   **场景建模**
        *   用况use-case
        *   部署图
        *   ……
    *   **类建模**
        *   **class图**
        *   **协作图**
        *   ……
    *   行为建模
        *   状态转换图
        *   活动图
        *   顺序图

# Chapter 09 设计工程

*   设计概念
    *   抽象abstraction
    *   体系结构architecture 
    *   模式patterns
    *   逐步求精refinement
    *   模块化modularity
    *   信息隐藏information hiding 
    *   模块独立functional independence 
    *   Refactoring（重构）

# Chapter 10 架构设计

*   为何进行体系结构设计

    *   体系结构是系统的表示形式, 使软件工程师能够:
        *   分析设计在满足其规定要求方面的有效性,
        *   在设计更改仍然相对容易的阶段考虑体系结构替代方案, 并且降低与软件建设相关的风险。

*   **体系结构风格（style）**

    *   Data-centered architecture
    *   Data flow architecture
    *   Call and return architecture
    *   Object-oriented architecture
    *   Layered architecture
    *   Each style describes a system category that encompasses:
        *   a set of components (e.g., a database, computational modules) that perform a function required by a system,
        *   a set of connectors that enable “communication, coordination, and cooperation” among components
        *   constraints that define how components can be integrated to form the system, and
        *   semantic models that enable a designer to understand the overall properties of a system.

*   *   Each style describes a system category that encompasses:
        *   a set of components (e.g., a database, computational modules) that perform a function required by a system,
        *   a set of connectors that enable “communication, coordination, and cooperation” among components,
        *   constraints that define how components can be integrated to form the system, and

    usemantic models that enable a designer to understand the overall properties of a system.

# Chapter 11 组件设计

*   构件
    *   A complete set of software components is defined during architectural design
    *   But the internal **data structures** and **processing details** of each component are not represented at a level of abstraction that is close to code
    *   Component-level design defines the **data structures**, **algorithms**, **interface characteristics**, and **communication mechanisms** allocated to each component
*   构建设计原则：开关、替换、依赖倒置、接口分离、内聚性、耦合性
*   构建设计方法：DPL、程序流程图、决策表

# Chapter 13~14 软件测试技术

*   Verification 确保软件正确实现功能
*   Validation 确保软件可追溯需求

*   测试策略
    *   **单元测试**
    *   **集成测试**：big bang, top down, bottom up
    *   确认测试
    *   系统测试
*   测试用例
*   测试技术
    *   白盒、黑盒
    *   手工测试、自动化测试
*   ![1547480427569](/Review/SoftwareEngineering/1547480427569.png)

# Chapter 15 软件产品度量

>   1.  软件质量
>
>   2.  一般来讲，**软件质量**是对明确陈述的功能和性能需求、明确记录的开发标准以及对所有专业化软件开发应具备的隐含特征的符合度。
>
>       *   软件需求是质量测量的基础，不符合需求就是没有质量。
>       *   若未能遵守开发准则，则肯定质量有问题。
>       *   若软件符合显示需求，但未能满足其隐式需求，则软件质量仍然值得怀疑。
>
>   3.  ![1547480657659](/Review/SoftwareEngineering/1547480657659.png)
>
>   4.  度量框架
>
>   5.  **软件产品**：文档、代码、软件等
>
>       **度量手段**: 
>
>       测度（measures）
>
>       度量（metrics）
>
>       指标（Indicators）
>
>       **度量原则**
>
>       >   •设定度量目标：The objectives of measurement should be established before data collection begins;
>       >
>       >   •定义要明确：Each technical metric should be defined in an unambiguous manner;
>       >
>       >   •有效理论支持：Metrics should be derived based on a theory that is valid for the domain of application (e.g., metrics for design should draw upon [利用] basic design concepts and principles and attempt to provide an indication of the presence of an attribute that is deemed desirable);
>       >
>       >   •度量指标的选择要是最合适的：Metrics should be tailored to best accommodate specific products and processes.
>
>       **度量过程**
>
>       >   •Formulation【公式化】. The derivation of software measures and metrics appropriate for the representation of the software that is being considered.
>       >
>       >   •Collection【收集数据】. The mechanism used to accumulate data required to derive the formulated metrics.
>       >
>       >   •Analysis【分析结果】. The computation of metrics and the application of mathematical tools.
>       >
>       >   •Interpretation【解释评估】. The evaluation of metrics results in an effort to gain insight into the quality of the representation.
>       >
>       >   •Feedback【反馈】 Recommendations derived from the interpretation of product metrics transmitted to the software team.
>
>       

*   过程度量作用：提供能够引导长期的软件过程改进的一组过程指标。 
    项目度量作用：使得软件管理者能够（1）评估正在进行中的项目的状态（2）跟踪潜在的分险（3） 在问题造成不良影响前发现他们（4）调整工作流程或任务（5）评估项目团队控制软件工作产品质 量的能力 
    产品度量作用：为分析、设计、编码和测试能更客观的执行和更定量的评估提供基础

# Chapter 21 软件工程管理

*   4P模型
    *   People — the most important element of a successful project
    *   Product — the software to be built
    *   Process — the set of framework activities and software engineering tasks to get the job done
    *   Project — all work required to make the product a reality

W5HH

![1547480963514](/Review/SoftwareEngineering/1547480963514.png)

# Chapter 15，22 过程和项目度量

![1547484904576](/Review/SoftwareEngineering/1547484904576.png)

*   **McCall质量因素**

*   >   Why measure
    >
    >   1.  assess the status of an ongoing project
    >   2.  track potential risks
    >   3.  uncover problem areas before they go “critical”
    >   4.  adjust work flow or tasks
    >   5.  evaluate the project team’s ability to control quality of software work products.

*   Measures,Metrics,Indicators

    *   process

    *   process product

    *   project

    *   1.  measures of **errors** uncovered before release of the software

        2.  **defects** delivered to and reported by end-users

        3.  work products delivered (**productivity**)

        4.  human **effort** expended

        5.  calendar **time** expended

        6.  **schedule** conformance

        7.  **other** measures.  

*   **度量的作用**

# Chapter 23 软件项目预算

*   **项目计划任务和内容**

    *   软件项目计划

        *   建立一套务实的策略控制跟踪监视一个复杂的技术项目
        *   目的：保证最终结果按时高质量。

    *   属性

        *   项目规模

        *   项目工作量（人月）

        *   项目所需资源

        *   项目成本

            ![1547485353292](/Review/SoftwareEngineering/1547485353292.png)

*   LOC&FP

*   ![1547485572230](/Review/SoftwareEngineering/1547485572230.png)

*   FP:Function Point 功能点

# Chapter 24 项目进度安排和跟踪



![1547485832089](/Review/SoftwareEngineering/1547485832089.png)

![1547485902823](/Review/SoftwareEngineering/1547485902823.png)

![1547485933135](/Review/SoftwareEngineering/1547485933135.png)

*   任务网络、关键路径的作用
*   ![1547486016026](/Review/SoftwareEngineering/1547486016026.png)
*   ![1547486047194](/Review/SoftwareEngineering/1547486047194.png)
*   **里程碑**

# Chapter 25 风险管理

>    风险具有不确定性和造成损失的特点

*   被动风险和主动风险管理
*   ![1547486476184](/Review/SoftwareEngineering/1547486476184.png)
*   Risk Management Paradigm（风险过程管理）
*   ![1547486503200](/Review/SoftwareEngineering/1547486503200.png)
*   ![1547486575631](/Review/SoftwareEngineering/1547486575631.png)
*   ![1547486606647](/Review/SoftwareEngineering/1547486606647.png)
*   **RMMM（Risk,Mitigation,Moritoring and Management）**
    *   mitigation:如何避免/转移风险
    *   monitoring:监视
    *   management:管理

# Chapter 26 质量管理

![1547488587407](/Review/SoftwareEngineering/1547488587407.png)

*   **McCall软件质量模型**
*   软件质量保证活动
*   ![1547488616231](/Review/SoftwareEngineering/1547488616231.png)
*   ![1547488508862](/Review/SoftwareEngineering/1547488508862.png)
*   **正式技术评审**
*   ![1547488739234](/Review/SoftwareEngineering/1547488739234.png)
*   ![1547488769582](/Review/SoftwareEngineering/1547488769582.png)
*   ![1547488790605](/Review/SoftwareEngineering/1547488790605.png)
*   ![1547488807766](/Review/SoftwareEngineering/1547488807766.png)
*   软件质量的成本

# Chapter 27 变更管理

![1547488863963](/Review/SoftwareEngineering/1547488863963.png)

*   软件配置项、版本、基线etc.
*   ![1547488886270](/Review/SoftwareEngineering/1547488886270.png)
*   ![1547488903866](/Review/SoftwareEngineering/1547488903866.png)
*   **软件配置管理流程**
*   ![1547488975691](/Review/SoftwareEngineering/1547488975691.png)