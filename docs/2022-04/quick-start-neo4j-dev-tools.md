# 手把手快速上手 Neo4j 开发和管理工具链

![](quick-start-neo4j-dev-tools/neo4j_graph_platform.jpg)

前面介绍了 Neo4j 图数据平台的概览，Neo4j已经从图数据库发展演变成完善的图数据平台，意味着：

1. 用户可以轻松地从不同的数据源将数据导入进Neo4j图数据库进行存储；
2. 为开发者提供了多种编程语言的客户端驱动来跟图数据存储交互；
3. 为DBA提供了丰富的工具和接口来运维数据库系统；
4. 为分析师和数据科学家，以及AI/ML（人工智能/机器学习）提供了业界最丰富的图算法和工具；
5. 为商业分析用户提供了便利的图数据探索和可视化工具；
6. 以及丰富的社区工具比如可视化、客户端驱动等等。

本文作为手把手系列，今天只能大概介绍一下相关的开发和管理工具链。我们开始吧。

## 数据导入

首先我们看一下数据导入的支持。

### 导入CSV文件

这是使用最多的数据格式，我们将准备好的数据转换成CSV，然后使用Cypher的`LOAD CSV`就可以将数据文件导入进Neo4j图数据库了。

LOAD CSV适合中小型的数据，量级在1000万条记录。

```cypher
LOAD CSV WITH HEADERS FROM 'file:///employees.csv' AS row
MERGE (e:Employee {employeeId: row.Id, email: row.Email})
WITH e, row
UNWIND split(row.Skills, ':') AS skill
MERGE (s:Skill {name: skill})
MERGE (e)-[r:HAS_EXPERIENCE]->(s)
```

### 从关系数据库导入数据

在实际应用中，大部分数据存在与现有的系统里，对于关系型数据库里的数据，也有不同的方式来将它们导入进Neo4j图数据库。可以使用APOC库的`apoc.load.jdbc`或使用ETL工具~~Kettle~~ Apache Hop，甚至是使用自己喜欢的编程语言和客户端驱动程序来开发特定的导入程序。

## 客户端驱动程序

除了Neo4j Browser作为日常数据库的管理工具，我们的数据通常是被其他程序系统来使用。

### Bolt协议

Neo4j提供了基于二进制的Bolt协议为应用程序使用，它基于 PackStream 序列化并通过证书支持 Cypher 类型系统、协议版本控制、身份验证和 TLS。对于 Neo4j 集群，Bolt 提供具有负载均衡和故障转移的智能客户端路由。

### 客户端驱动

Neo4j官方提供了多种编程语言驱动程序，包括[.NET](https://neo4j.com/developer/dotnet/)、[Java](https://neo4j.com/developer/java/)、[Spring](https://neo4j.com/developer/spring-data-neo4j/)、[JavaScript](https://neo4j.com/developer/javascript/)、[Go](https://neo4j.com/developer/go/)和[Python](https://neo4j.com/developer/python/)。还有来自社区的多种编程语言的驱动也可以使用。包括Ruby，PHP，C/C++，R等等。

### 在.NET程序里使用Neo4j

官方维护的Neo4j.Driver为.NET开发者提供，设计符合 .NET 的习惯，可以从nuget安装。适用于.NET Standards 2.0以上版本。

```
Install-Package Neo4j.Driver -Version 4.4.0
```

或：

```
dotnet add package Neo4j.Driver --version 4.4.0
```

就可以添加到项目里。

```csharp
IDriver driver = GraphDatabase.Driver("neo4j://localhost:7687", AuthTokens.Basic("username", "pasSW0rd"));
IAsyncSession session = driver.AsyncSession(o => o.WithDatabase("neo4j"));
try
{
    IResultCursor cursor = await session.RunAsync("CREATE (n) RETURN n");
    await cursor.ConsumeAsync();
}
finally
{
    await session.CloseAsync();
}

...
await driver.CloseAsync();
```

#### 详情参考

Nuget页面：https://www.nuget.org/packages/Neo4j.Driver

官方文档：https://neo4j.com/developer/dotnet/

GitHub代码库：https://github.com/neo4j/neo4j-dotnet-driver

### 在Java程序里使用Neo4j

有多种方式在Java程序里使用Neo4j，官方维护的驱动包可以通过Maven安装。适用于Java 8以上版本。

```xml
<groupId>org.neo4j.driver</groupId>
<artifactId>neo4j-java-driver</artifactId>
<version>4.4.4</version>
```

然后就可以使用了。

```java
Driver driver = GraphDatabase.driver("bolt://localhost:7687", AuthTokens.basic("neo4j", "PasSW0rd"));
try (Session session = driver.session()) {
    Result result = session.run("CREATE (n) RETURN n");
}
driver.close();
```

#### 详情参考

官方文档：https://neo4j.com/developer/java/

GitHub代码库：https://github.com/neo4j/neo4j-java-driver

### 使用Spring Data Neo4j

对于使用 Spring Framework 或 Spring Boot 并希望利用响应式开发原则的 Java 开发人员，可以通过 Spring Data Neo4j 集成 Neo4j。该库提供对 Neo4j 的便捷访问，包括对象映射、Spring Data 存储库、转换、事务处理、响应式支持等。

详情参考官方文档：https://neo4j.com/developer/spring-data-neo4j/

### 使用Neo4j OGM（对象图映射）

对于需要简单机制来使用 Neo4j 管理其领域对象的 Java 开发人员，可以使用 Neo4j 对象图映射 (OGM) 库。这是支持 Spring Data Neo4j 的同一个库，但也可以独立于 Spring 使用。

详情参考官方文档：https://neo4j.com/developer/neo4j-ogm/

### 在 JS 程序里使用Neo4j

任何应用最终都会有一个 NodeJS 的实现，（不知道谁说的）。Neo4j官方维护了给JavaScript开发者使用的客户端驱动。适用于常见的LTS版本。

```
npm install neo4j-driver
```

```js
// Create a driver instance, for the user `neo4j` with password `password`.
// It should be enough to have a single driver per database per application.
var driver = neo4j.driver(
  'neo4j://localhost',
  neo4j.auth.basic('neo4j', 'password')
)

// Close the driver when application exits.
// This closes all used network connections.
await driver.close()
```

#### 详情参考

官方文档：https://neo4j.com/developer/javascript/

GitHub代码库：https://github.com/neo4j/neo4j-javascript-driver

### 在Python程序里使用Neo4j

Python开发者们可以使用官方的客户端驱动连接到Neo4j示例，从此轻松地进行图数据管理和数据科学应用开发。适用于Python 3.5及以上版本。

```
pip install neo4j
```

例子一个：

```python
from neo4j import GraphDatabase

driver = GraphDatabase.driver("neo4j://localhost:7687", auth=("neo4j", "password"))

def add_friend(tx, name, friend_name):
    tx.run("MERGE (a:Person {name: $name}) "
           "MERGE (a)-[:KNOWS]->(friend:Person {name: $friend_name})",
           name=name, friend_name=friend_name)

def print_friends(tx, name):
    for record in tx.run("MATCH (a:Person)-[:KNOWS]->(friend) WHERE a.name = $name "
                         "RETURN friend.name ORDER BY friend.name", name=name):
        print(record["friend.name"])

with driver.session() as session:
    session.write_transaction(add_friend, "Arthur", "Guinevere")
    session.write_transaction(add_friend, "Arthur", "Lancelot")
    session.write_transaction(add_friend, "Arthur", "Merlin")
    session.read_transaction(print_friends, "Arthur")

driver.close()
```

#### 详情参考

官方文档：https://neo4j.com/developer/python/

GitHub代码库：https://github.com/neo4j/neo4j-python-driver

### 在Go程序里使用Neo4j

Go 开发者现在也可以使用官方维护的客户端驱动了。适用于Go 1.10。

```go
go get github.com/neo4j/neo4j-go-driver/v4
```

```go
package main

import (
    "fmt"
    "github.com/neo4j/neo4j-go-driver/v4/neo4j"
    "io"
    "log"
)

func main() {
	// Neo4j 4.0, defaults to no TLS therefore use bolt:// or neo4j://
	// Neo4j 3.5, defaults to self-signed certificates, TLS on, therefore use bolt+ssc:// or neo4j+ssc://
	dbUri := "neo4j://localhost:7687"
	driver, err := neo4j.NewDriver(dbUri, neo4j.BasicAuth("username", "password", ""))
	if err != nil {
		panic(err)
	}
	// Handle driver lifetime based on your application lifetime requirements  driver's lifetime is usually
	// bound by the application lifetime, which usually implies one driver instance per application
	defer driver.Close()
	item, err := insertItem(driver)
	if err != nil {
		panic(err)
	}
	fmt.Printf("%v\n", item)
}
```

#### 详情参考

官方文档：https://neo4j.com/developer/go/

GitHub代码库：https://github.com/neo4j/neo4j-go-driver

还有很多内容无法一一展开，请访问官方文档进行深入了解和选择你喜欢的编程语言开始使用。

## 图数据科学

Neo4j提供了超过60种的图算法，都集成在了图数据科学库中。支持图搜索，路径查找，中心度，社区检测，图嵌入等算法。

可以访问图数据科学页面了解更多。

## Neo4j实时数据和BI连接工具

Neo4j还提供连接到Apache Spark和Apache Kafka的连接器。以及到BI工具的连接器。

详情可以了解文档。

## DBA数据库系统运维工具

Neo4j Admin 是管理 Neo4j 实例的主要工具。它是作为产品的一部分安装的命令行工具，可以使用许多命令执行。一些命令在单独的部分中进行了更详细的描述。

Neo4j Admin 位于*bin*目录中，调用方式如下：

```
neo4j-admin [-hV] [COMMAND]
```

支持一般任务包括检查数据库的一致性，查看存储信息，查看页面缓存和内存设置信息，以及数据库的备份和复制等操作。它还支持导入大批量的数据。

详情参考

https://neo4j.com/docs/operations-manual/current/tools/neo4j-admin/

## 结束

本文算是一篇补充概览，欢迎继续探索Neo4j图数据平台。