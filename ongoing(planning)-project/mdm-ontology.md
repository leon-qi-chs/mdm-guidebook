# MDM Ontology (Knowledge Graph)

## 项目信息

| 项目属性 | 详情 |
|---------|------|
| **项目名称** | MDM Ontology |
| **项目类型** | 📋 Planning（规划中） |
| **当前状态** | 规划阶段 |
| **优先级** | 🟡 Medium |
| **预计开始** | Q3 2026 |

## 项目背景

构建 MDM 领域的本体（Ontology）和知识图谱，用于数据语义理解、数据关系发现和智能数据访问。

**核心目标:**
- 建立 MDM 数据本体模型
- 构建产品、客户、组织等实体知识图谱
- 实现基于知识图谱的数据访问和查询
- 支持数据关系发现和推理

## Use Case: 数据访问（Data Access）

### 传统数据访问的挑战

**问题:**
- 用户需要知道具体的表名、字段名
- SQL 查询门槛高，非技术人员难以使用
- 跨表关联复杂，容易写错
- 业务术语和技术字段映射困难

**示例困境:**
```
业务问题："SK-II 在中国的所有产品有哪些？"

传统方式需要：
1. 知道 Product 表在哪里
2. 知道 Brand 字段叫什么
3. 知道如何 JOIN Product_Region_Mapping 表
4. 写复杂的 SQL
```

### 基于 Ontology 的数据访问

**解决方案:**
- **自然语言查询** - "Show me all SK-II products in China"
- **语义理解** - 理解 "SK-II" = Brand, "China" = Country
- **自动生成 SQL** - 通过知识图谱映射到实际表和字段
- **关系推理** - 自动推断需要 JOIN 的表

**查询流程:**
```
自然语言 → 意图识别 → 实体抽取 → 知识图谱映射 → Graph SQL → 传统 SQL → 结果返回
```

### Graph SQL vs 传统 SQL

**传统 SQL:**
```sql
SELECT p.product_name, p.gtin
FROM Product p
JOIN Product_Region_Mapping prm ON p.product_id = prm.product_id
JOIN Region r ON prm.region_id = r.region_id
WHERE p.brand = 'SK-II' AND r.country = 'China'
```

**Graph SQL (Cypher-style):**
```cypher
MATCH (b:Brand {name: 'SK-II'})-[:HAS_PRODUCT]->(p:Product)-[:SOLD_IN]->(c:Country {name: 'China'})
RETURN p.name, p.gtin
```

**优势:**
- 更直观的关系表达
- 更少的 JOIN 语法
- 更容易理解的业务逻辑

## 技术架构

### Ontology 模型设计

**核心实体（Core Entities）:**
- **Product** - 产品
- **Brand** - 品牌
- **Customer** - 客户
- **Organization** - 组织
- **Location** - 地点
- **Category** - 分类

**关系（Relationships）:**
- Brand `HAS_PRODUCT` Product
- Product `SOLD_IN` Country/Region
- Product `BELONGS_TO` Category
- Customer `LOCATED_IN` Location
- Organization `OPERATES_IN` Country

**属性（Attributes）:**
- Product: GTIN, Name, Description, Launch Date
- Brand: Brand Code, Brand Name, Owner
- Customer: Customer ID, Name, Type, GLN

### 知识图谱存储

**技术选型:**
- **Neo4j** - 图数据库（推荐）
  - 原生图存储
  - Cypher 查询语言
  - 强大的关系查询能力
  
- **Azure Cosmos DB (Gremlin API)** - 云原生图数据库
  - Azure 集成
  - 全球分布
  - 弹性扩展

- **RDF Store (如 Stardog)** - 语义网标准
  - RDF/OWL 标准
  - SPARQL 查询
  - 推理能力

### 数据摄入流程

```
MDM Database → ETL (Property Graph) → Graph Database ← Ontology Schema
```

**数据转换:**
1. 从 MDM 关系型数据库提取数据
2. 根据 Ontology 模型转换为图结构
3. 实体识别和关系建立
4. 加载到图数据库

### 查询和访问层

**自然语言接口:**
- NLP 引擎（如 spaCy, BERT）
- 实体识别（NER）
- 意图分类
- SQL 生成

**Graph SQL API:**
- RESTful API
- 支持 Cypher/Gremlin 查询
- 查询优化
- 缓存机制

**传统 SQL 兼容:**
- Graph → SQL 翻译层
- 支持现有 BI 工具
- 性能优化

## 项目范围

### Phase 1: Ontology 设计（Q3 2026）
- [ ] 业务实体建模
- [ ] 关系定义
- [ ] 属性和约束
- [ ] Ontology 验证

### Phase 2: 试点实施（Q4 2026）
- [ ] 图数据库部署（Neo4j/Cosmos DB）
- [ ] Product + Brand 数据摄入
- [ ] Graph SQL API 开发
- [ ] 试点查询场景

### Phase 3: 扩展和优化（2027 H1）
- [ ] 全量 MDM 数据摄入
- [ ] 自然语言查询接口
- [ ] 性能优化
- [ ] BI 工具集成

## Required Skillset

### 图数据库技能

**Neo4j:**
- Cypher 查询语言
- 图数据建模
- 性能调优
- Clustering 和高可用

**Graph SQL (Cypher):**
```cypher
// 基础查询
MATCH (n:Product) RETURN n LIMIT 10

// 关系查询
MATCH (b:Brand)-[:HAS_PRODUCT]->(p:Product)
WHERE b.name = 'SK-II'
RETURN p.name, p.gtin

// 多跳关系
MATCH (b:Brand)-[:HAS_PRODUCT]->(p:Product)-[:SOLD_IN*1..3]->(c:Country)
RETURN b.name, p.name, c.name

// 聚合
MATCH (b:Brand)-[:HAS_PRODUCT]->(p:Product)
RETURN b.name, COUNT(p) AS product_count
ORDER BY product_count DESC
```

### 传统 SQL 技能

**SQL Server/PostgreSQL:**
- 复杂 JOIN 查询
- 子查询和CTE
- 窗口函数
- 性能优化

**数据建模:**
- 实体关系模型（ER Model）
- 星型/雪花模型
- 数据仓库设计

### 语义网技能（可选）

**RDF/OWL:**
- RDF 三元组（Subject-Predicate-Object）
- OWL 本体建模
- SPARQL 查询
- 推理引擎

### ETL 和数据工程

**数据转换:**
- Azure Data Factory
- Python (pandas, networkx)
- 图数据 ETL 最佳实践

**数据质量:**
- 实体消歧（Entity Resolution）
- 关系验证
- 数据完整性检查

## 关键指标

| 指标 | 目标 | 说明 |
|------|------|------|
| **查询响应时间** | ≤ 2s | Graph SQL 查询 |
| **自然语言查询准确率** | ≥ 80% | NLP 查询理解 |
| **数据同步延迟** | ≤ 1h | MDM → Graph DB |
| **图数据覆盖率** | 100% | 核心 MDM 实体 |

## 业务价值

**提升数据访问效率:**
- 非技术人员也能查询数据
- 减少 IT 支持请求
- 加速业务洞察

**数据关系发现:**
- 自动发现隐藏关系
- 影响分析
- 推荐系统基础

**数据治理:**
- 清晰的数据血缘
- 数据使用监控
- 合规性追踪

## 风险和挑战

**技术风险:**
- 图数据库学习曲线
- 性能调优复杂度
- 与现有系统集成

**数据风险:**
- 数据质量要求高
- 实体消歧难度大
- 关系准确性保障

**成本风险:**
- 图数据库授权成本（Neo4j Enterprise）
- 存储成本（数据双写）
- 维护成本

---

**文档版本:** v1.0  
**创建日期:** 2026-07-07  
**最后更新:** 2026-07-07
