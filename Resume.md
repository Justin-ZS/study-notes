# 个人信息
* 金子森/男/1995
* 本科/西安电子科技大学/信息安全
* 工作年限：4年
* 手机：18829209541
* Github：[https://github.com/Justin-ZS](https://github.com/Justin-ZS)

# 工作经历
## 西安葡萄城（2017年7月～2021年6月）
### 4年同一个项目（[商业智能](https://www.grapecity.com.cn/solutions/wyn)）核心开发
从零开始和项目一起成长,承担核心功能的预研，开发和迭代，包括 Advanced Chart, Dashboard Layout, Data Engine 等等

### Data Engin
前后一共三个版本
  * **V1**(已废弃): 后端简单聚合过滤 + 前端计算，负责前端计算引擎的迭代和维护
  * **V2**(使用中): 前端 JSON -> 后端 SQL，设计描述数据的JSON结构和对应的查询SQL语句,给出JSON到SQL的转化流程图，负责前端部分的重构和实现
  * **V3**(开发中): 前端 类DAX -> 后端 SQL，深入学习DAX，预研实现的可行性，提供落地的方向

### Dashboard Layout
从始至终负责仪表板所有布局功能的实现和迭代
* **难点**：功能复杂：Web端/移动端，不/允许重叠，手动/自动布局，不/吸附方格或图表，图表的分组，容器图表，等等
* **实现**：参考[最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)，提供高性能的单点移动布局；分离图表渲染和布局指示组件，减少布局期间的组件渲染和DOM修改，提升性能；将复杂的功能抽象为统一的配置接口，用一套代码提供所有的功能。

### Advanced Chart
负责高阶图表的设计和实现，高阶图表 = 图表 + 增强功能
* **设计**：灵感来源于高阶函数，为了提供高阶图表，需要把图表的生命周期升阶，包括的创建，获取数据，渲染等等。
* **实现**：在不侵入图表逻辑的前提下，尽可能复用已有代码，以统一的方式附加增强功能
* **例子**：计算图表：数据来源于聚集结果+类Excel公式；合并图表：数据来源于多个聚集结果的合并

# 技术栈
前端基础：JavaScript/TypeScript, CSS/Sass  
前端框架：React, Redux   
前端工程化：Webpack, ESLint  
后端语言：C#, NodeJS  
开发工具：Git, Docker, VSCode  
数据库：SQL  

# 自我评价
4年外企经历，拥有良好的英文阅读和写作能力  
多年敏捷开发经验，熟悉 Jira, Bitbucket, Confluence 工作流程  
追求简约的代码，优雅的实现，崇尚 Don't repeat yourself   
热爱技术，自学函数式编程，读过《SICP》，有良好的自驱力   
期望 Work Life Balance，期望成为专业的前端开发  