# SmartKG -> Neo4j



```cypher
LOAD CSV WITH HEADERS
FROM "file:///SmartKG_KGDesc_COVID19_zh-relationships.csv" AS line
MATCH (s), (t)
WHERE s.`实体id`=line.`源实体id` and t.`实体id`=line.`目标实体id`
MERGE (s)-[:REL{name:line.`关系类型`}]->(t)
```

