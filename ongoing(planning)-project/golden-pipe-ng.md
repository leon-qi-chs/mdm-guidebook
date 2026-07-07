# Golden PIPE NG (Next Generation Architecture)

## 项目信息

| 项目属性 | 详情 |
|---------|------|
| **项目名称** | Golden PIPE NG |
| **项目类型** | 📋 Planning（规划中） |
| **当前状态** | 规划阶段 |
| **优先级** | 🔴 High |
| **预计开始** | Q3 2026 |

## 项目背景

Golden PIPE 作为 MDM 核心数据管道，已运行 3+ 年，随着业务增长和技术演进，需要进行架构升级以应对新的挑战。

**当前挑战：**
- **性能瓶颈** - 数据量增长 10x，处理时间过长
- **技术债务** - 老旧技术栈，维护成本高
- **扩展性不足** - 新数据源集成困难
- **数据质量** - 缺乏端到端数据质量保障
- **实时性不足** - 批处理为主，无法满足实时需求

## 项目目标

**核心目标：**
- 🚀 **性能提升 5x** - ETL 处理时间缩短 80%
- ☁️ **Cloud-native 架构** - 全面云原生化
- 🔄 **实时+批处理** - Lambda 架构支持实时和批处理
- 📊 **数据质量** - 内置数据质量框架
- 🎯 **低代码化** - 配置化管道，减少开发工作量

## 架构设计

### 当前架构（Legacy）

```
Data Sources → SSIS Packages → SQL Server Staging → Stored Procedures → MDM Database
```

**问题：**
- SSIS 包难以维护
- SQL Server 计算能力受限
- 缺少数据质量检查
- 无法处理实时数据

### 目标架构（Next Generation）

```
                   Batch Layer (离线批处理)
Data Sources → Event Hub → Azure Data Factory → Delta Lake → Spark ETL → MDM DB
                             ↓
                   Speed Layer (实时流处理)
                             ↓
                      Stream Processing (Databricks Structured Streaming)
                             ↓
                      Serving Layer (Redis Cache + Cosmos DB)
```

**Lambda 架构：**
- **Batch Layer** - 每日全量/增量批处理（高质量、完整性）
- **Speed Layer** - 实时流处理（低延迟、近实时）
- **Serving Layer** - 统一查询层（合并批处理和实时视图）

### 核心技术栈

| 组件 | 当前技术 | NG 技术 | 优势 |
|------|---------|---------|------|
| **数据集成** | SSIS | Azure Data Factory | Cloud-native, 维护简单 |
| **数据存储** | SQL Server | Delta Lake (Databricks) | ACID + 大数据 + 时间旅行 |
| **ETL 引擎** | SQL Stored Proc | Spark (Python/Scala) | 分布式计算，性能 10x-100x |
| **实时处理** | 无 | Structured Streaming | 实时数据处理 |
| **数据质量** | Scattered | Great Expectations | 统一数据质量框架 |
| **编排** | SQL Agent | Azure Data Factory | 可视化编排，监控完善 |
| **Serving** | SQL Server | Cosmos DB + Redis | 全球分布 + 低延迟 |

## 详细设计

### 1. Batch Layer（批处理层）

#### 数据摄入

**Azure Data Factory Pipelines:**
- **Copy Activity** - 从源系统复制数据
- **Incremental Load** - CDC (Change Data Capture) 增量加载
- **Data Validation** - schema 验证、空值检查

**数据源集成：**
- SAP (OData/RFC)
- SQL Server (CDC)
- Oracle (Golden Gate)
- REST APIs
- File-based (CSV, JSON, Parquet)

#### Data Lake 存储（Delta Lake）

**分层架构：**
```
Raw Layer (Bronze) → Cleaned Layer (Silver) → Curated Layer (Gold)
```

**Bronze Layer（原始数据）：**
- 完整保留源系统数据
- Parquet 格式存储
- 分区策略：`/source={source_system}/date={yyyy-mm-dd}/`

**Silver Layer（清洗数据）：**
- 数据类型标准化
- 去重和数据清洗
- 历史数据保留（SCD Type 2）

**Gold Layer（业务数据）：**
- 业务视图（Dimension + Fact）
- 高性能查询优化
- 数据质量标签

#### ETL 处理（Databricks）

**Spark ETL Jobs:**
- **Python/PySpark** - 主要开发语言
- **Delta Live Tables** - 声明式 ETL
- **Business Logic** - 转换规则、数据enrichment

**性能优化：**
- **Partitioning** - 按日期/品牌分区
- **Z-Ordering** - 数据排序优化
- **Caching** - 中间结果缓存
- **Broadcast Join** - 小表广播

### 2. Speed Layer（实时处理层）

#### 实时数据流

**Event Hub → Databricks Streaming:**
- **Change Data Capture** - 捕获数据变更事件
- **Event Schema** - 标准化事件格式
- **Watermarking** - 处理延迟数据

**实时处理场景：**
- 产品价格更新（实时同步到电商）
- 库存状态变更（实时可见性）
- 产品上下架（实时反映）

#### Streaming ETL

**Structured Streaming:**
```python
from pyspark.sql import SparkSession
from delta.tables import *

# Read from Event Hub
stream_df = spark.readStream \
    .format("eventhubs") \
    .option("eventhubs.connectionString", connection_string) \
    .load()

# Transform
transformed_df = stream_df \
    .select("product_id", "price", "timestamp") \
    .withWatermark("timestamp", "10 minutes")

# Write to Delta Lake
query = transformed_df \
    .writeStream \
    .format("delta") \
    .outputMode("append") \
    .option("checkpointLocation", checkpoint_path) \
    .start(delta_table_path)
```

### 3. Serving Layer（服务层）

#### 统一查询接口

**API Gateway:**
- RESTful APIs
- GraphQL（可选）
- gRPC（高性能场景）

**查询路由：**
- 实时数据 → Cosmos DB / Redis
- 历史数据 → Delta Lake
- 合并视图 → 应用层聚合

#### Cosmos DB（实时数据）

**优势：**
- 全球分布，低延迟（< 10ms）
- 弹性扩展
- Change Feed（实时同步）

**数据模型：**
- Document-based
- 分区键：product_id / customer_id
- TTL 策略：实时数据保留 7 天

#### Redis Cache（热数据）

**使用场景：**
- 高频查询产品（Top 1000）
- Session 数据
- Lookup Tables

**缓存策略：**
- Cache-aside Pattern
- TTL: 1-24 小时
- 缓存预热

### 4. 数据质量框架

#### Great Expectations

**数据质量检查：**
- **Schema Validation** - 字段类型、必填检查
- **Value Validation** - 范围、格式、枚举值
- **Relationship Validation** - 外键、引用完整性
- **Business Rules** - 自定义业务规则

**Expectations 示例：**
```python
# Product table expectations
expect_column_values_to_not_be_null(column="product_id")
expect_column_values_to_be_unique(column="gtin")
expect_column_values_to_be_in_set(column="status", value_set=["Active", "Inactive"])
expect_column_mean_to_be_between(column="price", min_value=0, max_value=10000)
```

**质量报告：**
- 每日数据质量报告
- 异常数据自动告警
- 质量趋势分析

### 5. 数据编排（Orchestration）

#### Azure Data Factory

**Pipeline 组织：**
```
Master Pipeline
  ├── Ingestion Pipelines (并行执行)
  │     ├── SAP Ingestion
  │     ├── SQL Server CDC
  │     └── API Ingestion
  ├── Silver Layer ETL
  ├── Gold Layer ETL
  └── Data Quality Check
```

**调度策略：**
- 每日批处理：凌晨 2:00 AM
- 增量刷新：每小时一次
- 实时流：7x24 运行

**监控和告警：**
- Pipeline 执行状态
- 数据量异常检测
- 处理时间监控
- 失败自动重试

## 数据治理

### 数据血缘（Data Lineage）

**Azure Purview:**
- 自动捕获数据血缘
- 端到端可追溯（Source → Target）
- 字段级别血缘
- 影响分析

### 数据安全

**访问控制：**
- Azure AD 集成
- RBAC（Role-Based Access Control）
- Column-level Security
- Row-level Security

**数据脱敏：**
- PII 数据自动脱敏
- Dynamic Data Masking
- 生产 vs 非生产环境隔离

### 数据合规

**GDPR/PIPL 合规：**
- 个人数据标识
- 数据保留策略
- 数据删除机制
- 审计日志

## 迁移策略

### 迁移路径

**Phase 1: 基础设施搭建（2 个月）**
- [ ] Azure Data Factory 设置
- [ ] Databricks Workspace 创建
- [ ] Delta Lake 配置
- [ ] Event Hub 和 Cosmos DB 设置

**Phase 2: 试点迁移（2 个月）**
- [ ] 选择试点数据域（如 Product Master）
- [ ] 开发新 ETL Pipelines
- [ ] 双写运行（Legacy + NG）
- [ ] 数据一致性验证
- [ ] 性能对比

**Phase 3: 全面迁移（6 个月）**
- [ ] 分批迁移所有数据域
- [ ] 逐步下线 Legacy Pipelines
- [ ] 监控和优化
- [ ] 文档和培训

**Phase 4: Legacy 系统下线（1 个月）**
- [ ] 验证所有功能迁移完成
- [ ] Legacy 系统备份
- [ ] 正式下线

### 风险缓解

**数据一致性：**
- 双写验证期（至少 1 个月）
- 自动化数据对账
- A/B 测试

**性能风险：**
- 压力测试（Peak Load Test）
- 容量规划
- 自动扩展配置

**业务连续性：**
- 快速回滚机制（切回 Legacy）
- 分批迁移（降低风险）
- 7x24 支持团队

## 性能目标

| 指标 | 当前 | 目标 | 提升 |
|------|------|------|------|
| **Daily ETL 时间** | 6 小时 | 1 小时 | 5x |
| **增量刷新** | 不支持 | 15 分钟 | N/A |
| **实时延迟** | N/A | < 1 分钟 | 实时能力 |
| **并发查询** | 100 QPS | 1000 QPS | 10x |
| **数据质量** | 手工检查 | 自动化 100% | 全覆盖 |

## 成本分析

### 月度成本估算

| 组件 | 成本（月） | 备注 |
|------|----------|------|
| **Databricks** | $8,000 | 计算集群 |
| **Delta Lake Storage** | $2,000 | ADLS Gen2 存储 |
| **Azure Data Factory** | $1,500 | Pipeline 执行 |
| **Event Hub** | $500 | 实时流 |
| **Cosmos DB** | $3,000 | 实时数据存储 |
| **Redis Cache** | $500 | 缓存 |
| **其他** | $500 | 监控、网络等 |
| **总计** | **$16,000/月** | ≈ $192,000/年 |

### 成本优化

**节省策略：**
- Databricks Spot Instances（节省 60-80%）
- Reserved Capacity（节省 30-50%）
- 数据生命周期管理（Hot → Cool → Archive）
- 自动 Scaling Down

**预期节省：**
- 通过优化，成本可降至 $10,000-12,000/月

## 关键指标

| 指标 | 目标 | 说明 |
|------|------|------|
| **ETL 性能提升** | 5x | 处理时间缩短 |
| **实时延迟** | < 1 分钟 | 新能力 |
| **数据质量** | 100% | 自动化覆盖 |
| **查询性能** | 10x | API 响应时间 |
| **系统可用性** | 99.9% | SLA 目标 |

## 技能要求

**团队技能提升：**
- **PySpark** - ETL 开发
- **Delta Lake** - 数据湖管理
- **Databricks** - 平台使用
- **Azure Data Factory** - Pipeline 编排
- **Streaming** - 实时数据处理

**培训计划：**
- Databricks 官方培训（2 周）
- PySpark 实战培训（1 周）
- Delta Lake 培训（1 周）
- 内部知识分享

---

**文档版本:** v1.0  
**创建日期:** 2026-07-07  
**最后更新:** 2026-07-07
