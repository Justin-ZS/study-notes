# 个人信息
* 金子森/男/1995
* 本科/西安电子科技大学/信息安全
* 工作年限：7年
* 手机：18829209541
* 求职意向：前端高级/资深开发工程师
* 主技术栈：JS/TS，React（无Vue/移动端/小程序经验）

# 工作经历
## 西安葡萄城(GrapeCity)（4年，2017年7月～2021年6月）
### [Wyn BI](https://www.grapecity.com.cn/solutions/wyn) - Dashboard组 - 核心开发工程师
整整4年(加实习是5年)都在同一个组，从零开始和项目一起成长，承担核心功能的预研，开发和迭代，包括 Data Engine，Dashboard Layout，Advanced Chart 等等

### Data Engine
前后一共三个版本
  * **V1**(已废弃): 后端简单聚合过滤 + 前端计算，负责前端计算引擎的迭代和维护
  * **V2**(21年发布): 前端 JSON -> 后端 SQL，设计描述数据的 JSON 结构和对应的查询SQL语句,给出 JSON 到 SQL 的转译流程图，负责前端部分的重构和再实现
  * **V3**([现已发布](https://www.grapecity.com.cn/solutions/wyn/help/docs/wax/recognize-wax)): 前端 类DAX -> 后端 SQL，深入学习DAX，预研实现的可行性，提供落地的方向

### Dashboard Layout
从始至终负责仪表板所有布局功能的实现和迭代
* **难点**：功能复杂：Web端/移动端，不/允许重叠，手动/自动布局，不/吸附方格或图表，Container/Group，等等
* **实现**：参考[最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)，提供高性能的单点移动布局；分离图表渲染和布局指示组件，减少布局期间的组件渲染和DOM修改，提升性能；将复杂的功能抽象为统一的配置接口，用一套代码提供所有的功能。

### Advanced Chart
负责高阶图表的设计和实现，高阶图表 = 图表 + 增强功能
* **设计**：灵感来源于高阶函数，为了提供高阶图表，需要把图表的生命周期升阶，包括的创建，获取数据，渲染等等
* **实现**：在不侵入图表逻辑的前提下，尽可能复用已有代码，以统一的方式附加增强功能
* **例子**：计算图表：数据来源于聚集结果+类Excel公式；合并图表：数据来源于多个聚集结果的合

## 微策略(MicroStrategy)（3年，2021年9月～2024年9月）
### [MSTR BI](https://www.microstrategy.com/enterprise-analytics/dossier) - 可视化/Visualization组 - 高级开发工程师
负责维护和迭代可视化/Viz 相关的功能，[AI Chatbot](https://www.microstrategy.com/enterprise-analytics/ai-chatbot-for-apps) 中 Viz 的渲染，[Auto Narratives](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-smart-narrative) 中算法推荐 Prompts 的撰写

### Viz 的维护和迭代
主要是维护自研的前端框架 - Mojo(ES5时代的产物)，为 Viz 添加新的功能，例如 Viz Theme，Copy/Paste Viz Formatting 等  
此外还维护了基于 React 的 Viz 格式面板和 IOS 上的部分 Viz 的功能（Objective C）
* **难点**：理解高度复杂，代码量巨大，前后端完全耦合且多平台复用的 CodeBase，尽量在不引入 Regression 的前提下开发新功能和修复 Bug

### [AI Chatbot](https://www.microstrategy.com/enterprise-analytics/ai-chatbot-for-apps) 中 Viz 的渲染
AI Chatbot 是 MSTR 拥抱 AI 之后最大的核心功能，也是最大的卖点
* **实现**：将 ES5 时代的老框架代码封装成独立的 Visualization 组件，并增加部分响应式功能
* **难点**：将复杂又沉重的老框架封装成可复用的组件，在新的多实例场景下避免内存泄漏

### [Auto Narratives](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-smart-narrative) 中的 Prompts 
Auto Narratives 是 MSTR 2024 年最重磅的更新，暂时没找到官方的文档，毕竟刚做完就被裁了  
链接里的是 Power BI 的 Smart Narrative，功能类似，当初也是为了对标它才做的 Auto Narratives  
* **实现**：撰写 Prompts 让 AI 根据用户的需求和所选择的 Viz 的数据，推荐合适的算法和参数  
* **难点**：Example 结合 Step by Step 让 AI 更准确的理解用户的问题  
转译特殊字符，移除不必要的 ID 等来提高输出的稳定性和质量  
[调优 LLM 参数](https://platform.openai.com/docs/advanced-usage/frequency-and-presence-penalties)以避免 GPT 在生成结构化的输出时陷入死循环  

# 技术栈
前端基础：JavaScript/TypeScript, CSS/Sass  
前端框架：React, Redux   
前端工程化：Webpack, ESLint  
其他：SQL, C#, NodeJS，Objective C, AI Prompts  
开发工具：Git, VSCode  

# 自我评价
7年外企工作经历，拥有良好的英文阅读和写作能力  
多年敏捷开发经验，熟悉 Git/Jira/Github/Bitbucket/Confluence/Rally 工作流程  
力求代码简约，实现优雅，崇尚 Don't repeat yourself   
热爱技术，自学函数式编程，读过《SICP》，有良好的自驱力   
推崇 Work Life Balance  
