# 个人信息
* 金子森/男/1995
* 本科/西安电子科技大学/信息安全
* 工作年限：7年
* 手机：18829209541
* 求职意向：前端高级开发

# 工作经历
## 西安葡萄城(GrapeCity)（4年，2017年7月～2021年6月）
### [Wyn BI](https://www.grapecity.com.cn/solutions/wyn) - Dashboard组 - 核心开发
从零开始和项目一起成长，承担核心功能的预研，开发和迭代，包括 Advanced Chart, Dashboard Layout, Data Engine 等等

### Data Engin
前后一共三个版本
  * **V1**(已废弃): 后端简单聚合过滤 + 前端计算，负责前端计算引擎的迭代和维护
  * **V2**(21年发布): 前端 JSON -> 后端 SQL，设计描述数据的JSON结构和对应的查询SQL语句,给出JSON到SQL的转化流程图，负责前端部分的重构和实现
  * **V3**([现已发布](https://www.grapecity.com.cn/solutions/wyn/help/docs/wax/recognize-wax)): 前端 类DAX -> 后端 SQL，深入学习DAX，预研实现的可行性，提供落地的方向

### Dashboard Layout
从始至终负责仪表板所有布局功能的实现和迭代
* **难点**：功能复杂：Web端/移动端，不/允许重叠，手动/自动布局，不/吸附方格或图表，Container/Group，等等
* **实现**：参考[最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)，提供高性能的单点移动布局；分离图表渲染和布局指示组件，减少布局期间的组件渲染和DOM修改，提升性能；将复杂的功能抽象为统一的配置接口，用一套代码提供所有的功能。

### Advanced Chart
负责高阶图表的设计和实现，高阶图表 = 图表 + 增强功能
* **设计**：灵感来源于高阶函数，为了提供高阶图表，需要把图表的生命周期升阶，包括的创建，获取数据，渲染等等。
* **实现**：在不侵入图表逻辑的前提下，尽可能复用已有代码，以统一的方式附加增强功能
* **例子**：计算图表：数据来源于聚集结果+类Excel公式；合并图表：数据来源于多个聚集结果的合

## 微策略(MicroStrategy)（3年，2021年9月～2024年9月）
### MSTR BI Dashboard - 可视化/Visualization组 - 高级开发
负责维护和迭代 可视化/Viz 相关的功能，[AI Chatbot](https://www.microstrategy.com/enterprise-analytics/ai-chatbot-for-apps) 中 Viz 的渲染，[Auto Narratives](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-smart-narrative) 中算法推荐 Prompts 的撰写

### Viz 的维护和迭代
主要是基于自研前端框架 - Mojo(ES5时代的产物)，为 Viz 添加新的功能，例如 Viz Theme，Copy/Paste Viz Formatting 等。
此外还维护了一个基于 React 的 Viz Format Panel。
难点：理解高度复杂，代码量巨大，前后端完全耦合的 CodeBase，并在不引入 Regression 的前提下开发新功能和修复 Bug。

### [AI Chatbot](https://www.microstrategy.com/enterprise-analytics/ai-chatbot-for-apps) 中 Viz 的渲染

### [Auto Narratives](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-smart-narrative) 中的 Prompts 
没找到 MSTR 相关文档，毕竟刚做完就被开了。。链接里的是 Power BI 的 Smart Narrative，当初也是为了对标它才做的 Auto Narratives。



# 技术栈
前端基础：JavaScript/TypeScript, CSS/Sass  
前端框架：React, Redux   
前端工程化：Webpack, ESLint  
其他：SQL, C#, NodeJS，Objective C, AI Prompts  
开发工具：Git, VSCode  

# 自我评价
7年外企经历，拥有良好的英文阅读和写作能力  
多年敏捷开发经验，熟悉 Git/Jira/Bitbucket/Confluence/Rally 工作流程  
力求代码简约，实现优雅，崇尚 Don't repeat yourself   
热爱技术，自学函数式编程，读过《SICP》，有良好的自驱力   
推崇 Work Life Balance  
