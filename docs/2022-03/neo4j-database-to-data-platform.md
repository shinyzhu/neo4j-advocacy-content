# Neo4j 不仅是数据库，而且是统一的图数据平台

![img](neo4j-database-to-data-platform/banner-developer-blog.jpg)

## Neo4j不只是图数据库

如果我告诉你Neo4j不只是一个图数据库，你相信吗？

Neo4j是由2007年成立于瑞典的同名公司发布的开源图数据库产品。15年过去了，它还是一个图数据库吗？

![](neo4j-database-to-data-platform/Neo4j Under The Hood - Let’s Get Started!-TO00ELu_uJI-0001.png)

答案就是：

## Neo4j是图数据平台

![neo4j graph platform](neo4j-database-to-data-platform/neo4j_graph_platform.jpg)

当然，Neo4j的核心产品仍然是图数据库，同时提供了开源社区版和企业版。

但随着数据的多样性关联性不断丰富，以及业务需求的不断变化，我们处理的数据复杂性和量级都在不断增长。Neo4j从图数据库演变成图数据平台，也是在不断丰富产品本身和扩展交互能力，力求满足当前对于关联数据的丰富需求。

不同的角色都可以通过Neo4j图数据平台开展工作，除了开发者和DBA以外，还有业务分析人员、数据分析师、数据科学家，以及大数据工程师等，都可以通过他们熟悉的工具和技术，来实现基于Neo4j图数据平台的工作。

我们一起来看看Neo4j图数据平台包含哪些组件。

### 原生图存储的图数据库

原生图存储的核心图数据库是Neo4j图数据平台的基础，为交易（OLTP）和分析（OLAP）工作负载提供企业规模和性能、安全性和数据完整性。

### 数据科学和分析

提供探索工具 Neo4j Bloom、丰富的图数据科学算法库（GDS Library）和集成了有监督机器学习框架。

最近，Neo4j GDS刚刚发布2.0版本，总共提供了超过60种图算法。

### 开发工具和框架

提供完善的工具链、API、查询生成器、多种编程语言支持，以满足开发、管理、建模和快速原型设计的需要。

### 数据探索和可视化

为数据科学家、开发人员和分析师提供无代码的查询、数据建模和数据探索工具，并提供直观的可视化展现。

### 图查询语言支持 

包含Cypher和开源的openCypher，Neo4j正在领导参与图查询语言的国际标准化工作（GQL），以建立图查询的通用语言。

### 技术生态和系统集成

提供丰富的技术生态和集成商合作伙伴。包含数据摄取工具（JDBC、Kafka、Spark、BI工具等）来满足批量数据和流式数据导入的需求。

### 在任何地方运行

通过云厂商（AWS、GCP、Azure、阿里云等）的应用市场可作为服务部署或完全自主托管，或在企业本地部署。提供了非常灵活的部署方案。

## Neo4j和开源

那么怎样开始呢？首先从核心的产品-图数据库开始，Neo4j整套图数据平台都按照开源来分发，我在这里做了一个整理，也附上了GitHub的地址，别忘了加个星星。

![Neo4j Open Source](neo4j-database-to-data-platform/Neo4j Open Source.png)

社区版开源代码库地址：

`github.com/neo4j/neo4j`

GDS开源代码库地址：

`github.com/neo4j/graph-data-science/`

openCypher开源代码库地址：

`github.com/opencypher`

Java客户端驱动开源代码库：

`github.com/neo4j/neo4j-java-driver`

JavaScript客户端驱动开源代码库：

`github.com/neo4j/neo4j-javascript-driver`

Python客户端驱动开源代码库：

`github.com/neo4j/neo4j-python-driver`

.NET客户端驱动开源代码库：

`github.com/neo4j/neo4j-dotnet-driver`

Go客户端驱动开源代码库：

`github.com/neo4j/neo4j-go-driver`

JDBC驱动开源代码库：

`github.com/neo4j-contrib/neo4j-jdbc`

APOC开源代码库：

`github.com/neo4j-contrib/neo4j-apoc-procedures`

## 结束语

希望通过本文让你对Neo4j图数据平台有一个全面的印象，也能够找到立即开始体验的起点。

如有问题欢迎加入合作的社区，或直接留言发送消息联系我们。