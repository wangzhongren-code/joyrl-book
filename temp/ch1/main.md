---
bibliography: [../ref.bib]
---

## 绪论

### 强化学习概述
**强化学习（reinforcement learning，RL）**是广义机器学习的一个领域，注意这里广义机器学习泛指涵盖几乎所有人工智能方法的领域，包括狭义的机器学习（例如线性模型，支持向量机等）和深度学习等等。强化学习涉及智能体如何在环境中采取动作以便最大化累积奖励的概念。智能体在学习的过程中不会被预先告知应该采取何种动作，而是通过**试错（trial-and-error）**去发现采取哪种动作会使得对应的奖励最大。举一个典型的例子，就好比学生在学校学一门课，经验事实告诉我们最终的目标是在期末考试拿到尽可能高的分数。学生在这个过程中充当智能体，采取的动作或策略可以是中规中矩的上课听讲并按时完成作业，也可以再花点力气预习内容并上课认真听讲按时完成作业，还可以是不上课每天尽情玩乐，而最终期末考试分数就是一个奖励，决定最终奖励高低的则取决于采取动作或者学习策略的最优程度，当然实际的问题往往要复杂得多。此外智能体采取的动作也往往会影响到环境的状态和**即时奖励（immediate reward）**，比如我们学习了一天的功课之后脑袋会感到充实会有成就感，而我们尽情玩乐了一天之后会获得短暂的快乐并伴随着一定的空虚感，或者说上课认真听讲的同学会发现老师的态度会比较友好，不遵守课堂纪律的同学会受到老师的批评等等都说明采取的动作会影响到环境的状态。而即时奖励则与**长期奖励（long-term）**相对，其中期末考试的分数可以看做长期奖励，比如我们学习了一天会潜在地影响到最终的期末分数但是不能看到立竿见影的效果。我们能迅速看到的效果是学习了一章的内容然后去做对应章节的作业，作业的评分就相当于即时奖励，再比如我们玩了一天的游戏也能享受肾上腺素和多巴胺带来的生理性快乐，这也可以看作即时奖励。并且即便是玩了一天的游戏对于期末分数也就是长期奖励的影响也是微乎其微的，也许某位同学在学期第一天玩了一天的游戏之后痛定思痛并在接下来的每一天刻苦学习，那么他很大概率也是能获得比较高的最终奖励的。这就是强化学习除了试错之外的一个重要特性，即**延时奖励（delayed reward）**。注意试错是相对于最终目标而言的，比如我们知道如果平时作业得高分就说明这段时间学习效果好，一直作业得高分下去期末分数必然也会很高，因此相对于期末分数这个最终目标来说平时作业的高分是一种即时奖励。如果我们玩了一天的游戏，虽然我们收获了快乐，获得了生理性的即时奖励，但这种生理性的即时奖励相对于最终目标往往是起到反作用的，而这种情况下我们的平时作业分会比较差，相当于得到了一个比较低甚至是负数的奖励（也就是惩罚）。我们通过这些即时奖励或惩罚来判断我们每一步的动作或策略是否错误，然后一步一步地直至达到最终的目标，这就是试错的涵义。

强化学习是一个强交互过程，擅长解决序列决策问题。严格来讲，强化学习隶属于动态系统理论的范畴，涉及马尔可夫决策问题（序列决策问题的数学描述）的最优化控制，这点在后面马尔可夫过程相关章节中会详细展开。在这个最优化控制过程中，智能体必须能够感知到环境的状态并采取合适的动作，对应的动作又会影响到环境的状态，而能够解决这一类问题的方法我们都统称为强化学习方法。

### 强化学习与监督学习

强化学习是三种基本机器学习范式之一，另外两种是监督学习与无监督学习。如图1.1所示，监督学习是从一堆打好标签或者标注的训练数据中来学习的，这些标签通常由经验丰富的专家来确定，表示在某种环境状态下智能体应当采取的动作，更确切地说，是表示某种环境状态对应的分类，然后推断一些在训练集中没有出现的状态。但是在交互问题中打标签是不切实际的，在一些未知的状态下，智能体必须学会从自身的经验中学习，这就是强化学习的独特之处。就好比我们吃西瓜，假设在我们对西瓜没有任何先验认知的情况下，即前面所说的未知的状态或领域，我们是不知道哪种西瓜更好吃的。这种情况下，我们只能通过尝试来确定哪种西瓜好吃，好比神农尝百草一样，我们是尝百瓜之后才能练就一身仅通过西瓜外观就能判断西瓜好不好吃的本领，这个本领的学习过程是需要强化学习的。在我们成为吃瓜老手也就是经验丰富的专家之后，我们会总结一些经验，比如瓜皮颜色较深的、瓜瓤很红的、瓜敲起来声音较清脆往往会比较好吃，把瓜皮、瓜瓤颜色、敲瓜声音作为输入特征，把好吃与不好吃作为输出分类，这就是监督学习所做的事情。

<div align=center>
<img width="800" src="./figs/supervise_learning.png"/>
</div>
<div align=center>图 1.1 强化学习示意</div>

### 强化学习与无监督学习

无监督学习旨在发现数据样本的一些潜在特征然后将这些样本自动聚类，例如一个无监督学习在“吃瓜”的过程中也许发现了通过瓜皮、瓜瓤颜色、敲瓜声音这些特征能够将西瓜分成不同的几类，然后将它们划分开来。乍一看，无监督学习似乎跟强化学习很类似，因此有时候强化学习也被误解为一种无监督学习模型。但强化学习本质上其最终目标是最大化智能体在交互过程中获得的累积奖励而不是挖掘样本的潜在特征，尽管在智能体获得的经验中挖掘潜在特征会有所帮助，但是无监督学习同监督学习一样，本身并不具备处理交互问题的能力。

### 强化学习的例子

我们再举一些例子来帮助读者加深对强化学习的理解，并且读者们仔细想想也会发现生活中处处是强化学习。比如一个围棋下棋时，每次落子前都会在脑中演练接下来十几步可能出现的棋局，然后做出最佳的落子选择，然后当对手落完子之后会自我反馈对手的落子是否在计算之内，如果在计算之内那么又重复演练进行下一个落子，如果不在就可能学习到了新的经验以便提高自己的棋力。顺便提一句，其中演练的过程我们在强化学习中通常称之为规划（planning），不在计算之内然后学习新经验的情况称之为学习（learning），具体细节我们会在后面的章节中展开说明。再比如一个高级扫地机器人有时会决策是否到下一个房间继续打扫垃圾，还是返回到充电口处自行充电。这种决策主要基于它对自身现有电量的判断以及根据已有的经验（之前走过的房间和观测到充电口的位置等等）来分析如何以最快最方便的路径返回到充电口处充电，这就是强化学习会把探索过的状态等形成经验从而便于未来做出最优的决策。再比如我们开车从杭州到上海，通常会打开电子地图导航，到了一段高速导航会提示应当走哪条道保持多少速度，到了一个路口会提示左转右转，遇到一些新修的路面时，电子地图往往未来得及同步数据会给出错误的提示，此时就需要结合我们自身的经验来判断如何行驶了。这个例子说明我们在与环境互动的过程中会持续收到反馈，当出现一些未知的情形时，我们可以同样可以利用学习到的经验来决策，甚至是结合一些非机器学习的经验判断，比如我们知道红灯停绿灯行，此时我们可以不需要强化学习模型学出来这个策略，直接根据规则或其他的模型判断决策即可，这就是强化学习的灵活之处，并且常常在一些复杂的强化学习智能竞赛中运用到这项技巧（trick）。

### 强化学习的历史

在Sutton教授等人的书[@rl_intro]中讨论了强化学习历史形成的三条线：1）通过试错学习；2）最优化控制问题；3）时序差分（Temporal Difference）方法。这三条时间线最后在1980年代汇聚一堂，开始形成我们今天所讨论的强化学习概念雏形。由于 Sutton 教授在他的书中已经讲述得比较清楚，加之这也不是本书的重点内容，因此这里会简要罗列一下这三条时间线中的一些重要节点，感兴趣的读者可以在茶余饭后查阅相关资料对强化学习历史进行深入的考究。

#### 试错学习 


前面讲到，试错学习作为强化学习的重要特征之一，是具有一段独立的历史故事的。试错学习最初由\upcite{Thorndike:1911}等人（1911）受到生物学启发定义，举个简单的例子，把蚯蚓放在包含一条分岔口的盒子里，蚯蚓只能向左或向右爬行，在左边放上食物，在右边放上释放弱电的设备，当蚯蚓往左爬的时候获取食物即奖励，往右爬的时候会被电击受到惩罚，每次受到奖励或惩罚之后把蚯蚓重新放回起始点并继续向左或向右爬行。一开始蚯蚓可能没有意识到左边会拿到奖励右边会受到惩罚这一点，每次都会随机往两边爬行，直到累积足够的次数或经验之后蚯蚓就学会了每次只往左边爬行了，这个过程就是试错的过程，在我们的生活中屡见不鲜。试错概念后面在1961年被 Minsky 等人\upcite{Minsky:1961:ire}引入到机器学习模拟（Machine Learning analogue）中并用于处理信用分配问题（Credit Assignment Problem），其中信用分配问题也是目前强化学习研究的问题分支之一。与此同时，试错的计算过程也被泛化到了模式识别问题中（https://dl.acm.org/doi/abs/10.1145/1455292.1455309），主要利用试错信息来帮助监督学习更新相关参数。

TODO https://towardsdatascience.com/reinforcement-learning-fda8ff535bb6#5554


## 强化学习的应用}
讲一些现代应用，游戏、推荐系统、派单、组合优化、自动驾驶等等
## 强化学习要素
### 智能体与环境
### 策略
### 奖励
## 学习与规划
## 探索与利用
## 实战：学会用 VS Code 开发强化学习算法
###  VS Code 安装
###  VS Code 搭建Python环境
## 关键词
## 习题
## 面试题
## 本章小结


