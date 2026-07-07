# MDM Agent (Vibe Coding Service)

## 项目信息

| 项目属性 | 详情 |
|---------|------|
| **项目名称** | MDM Agent |
| **项目类型** | 🔄 Ongoing（持续运营） |
| **当前状态** | 进行中 |
| **优先级** | 🔴 High |

## 项目背景

将 Golden PIPE 后端能力打包成 Agent 服务，让 PO（Product Owner）能够快速进行 CICD（持续集成/持续部署），将原型实现为可直接进行 Vibe Coding 的服务。

**核心目标：**
- 封装 Golden PIPE 核心能力为 Agent API
- 简化 PO 的开发和部署流程
- 支持快速原型验证（Vibe Coding）
- 降低技术门槛，提升业务敏捷性

## 什么是 Vibe Coding？

**Vibe Coding** 是一种快速原型开发方式：
- **低代码/无代码** - PO 通过配置和 Agent 交互完成开发
- **快速迭代** - 从想法到原型只需数小时
- **即时反馈** - 实时预览和调试
- **自动化部署** - 一键 CICD 部署到测试环境

## 为什么需要 MDM Agent？

### 当前痛点

**开发流程复杂：**
- PO 有业务想法，但需要开发团队实现
- 从需求 → 开发 → 测试 → 部署 周期长（2-4周）
- 简单的数据查询和 API 都需要走完整开发流程

**原型验证困难：**
- 验证一个想法成本高
- 无法快速试错
- 错过最佳业务时机

**技术門槛高：**
- PO 需要了解 ETL、SQL、API 开发
- 依赖开发团队资源
- 创新受限于技术能力

### 解决方案：MDM Agent

**Golden PIPE 能力服务化：**
```
Golden PIPE 后端能力 → Agent API → PO 自助使用
```

**核心能力封装：**
- 数据查询 Agent
- 数据转换 Agent
- 数据质量 Agent
- API 生成 Agent
- CICD Agent

## 技术架构

### Agent 服务架构

```
PO（自然语言/配置界面）
        ↓
MDM Agent Gateway
        ↓
    Agent Pool
        ├── Data Query Agent（数据查询）
        ├── Data Transform Agent（数据转换）
        ├── Data Quality Agent（数据质量）
        ├── API Generator Agent（API 生成）
        └── CICD Agent（自动部署）
        ↓
Golden PIPE Backend（核心能力）
        ↓
    Data Lake
```

### 核心 Agent 能力

#### 1. Data Query Agent

**功能：**
- 自然语言转 SQL
- SQL 安全验证（防止危险操作）
- 查询结果预览
- 数据导出（CSV/JSON/Excel）

**使用示例：**
```
PO Input: 
"查询 SK-II 在中国过去 3 个月的销售数据，按月份汇总"

Agent Output:
✅ SQL 查询已生成
✅ 安全检查通过
📊 查询结果预览（前10行）
💾 导出选项：CSV | JSON | Excel
```

**技术实现：**
- Azure OpenAI (GPT-4) - 自然语言理解
- Text-to-SQL Model - SQL 生成
- SQL Parser - 安全验证
- Query Executor - 查询执行

#### 2. Data Transform Agent

**功能：**
- 数据清洗规则配置
- 数据转换逻辑定义
- 数据enrichment（数据增强）
- 自动生成 Spark ETL 代码

**使用示例：**
```
PO Input:
- 数据源：Product_Raw 表
- 转换规则：
  * Brand 字段统一大小写
  * 删除重复 GTIN
  * 补全缺失的 Category

Agent Output:
✅ 转换规则验证通过
✅ Spark ETL 代码已生成
✅ 数据质量预估：95%
🚀 一键部署到 DEV 环境
```

#### 3. Data Quality Agent

**功能：**
- 数据质量规则定义
- 自动化质量检查
- 异常数据报告
- 修正建议

**使用示例：**
```
PO Input:
- 检查表：Product 表
- 规则：GTIN 必须是 13 位数字，Price 必须 > 0

Agent Output:
📊 质量检查报告：
- 总记录数：10,000
- 通过率：98.5%
- 异常记录：150 条
📋 异常详情：
  - GTIN 格式错误：100 条
  - Price 为负数：50 条
💡 修正建议已生成
```

#### 4. API Generator Agent

**功能：**
- 基于数据表自动生成 RESTful API
- API 文档自动生成（Swagger）
- 认证授权配置
- API Gateway 集成

**使用示例：**
```
PO Input:
- 表名：Product
- API 类型：CRUD (Create, Read, Update, Delete)
- 权限：Read（公开），Write（需认证）

Agent Output:
✅ API 已生成：
  - GET    /api/products
  - GET    /api/products/{id}
  - POST   /api/products   (需认证)
  - PUT    /api/products/{id}  (需认证)
  - DELETE /api/products/{id}  (需认证)
📄 Swagger 文档：http://api-dev.mdm.com/swagger
🔐 认证配置：Azure AD
🚀 部署到 DEV：一键部署
```

**技术实现：**
- Code Generator - 自动生成 .NET/Python API
- OpenAPI Spec Generator - Swagger 文档
- Azure API Management - API Gateway
- Azure AD - 认证授权

#### 5. CICD Agent

**功能：**
- 一键部署到 DEV/SIT/UAT
- 自动化测试
- 回滚机制
- 部署状态监控

**使用示例：**
```
PO Input:
- 部署目标：DEV 环境
- 部署内容：Product API v1.2

Agent Output:
🔄 部署进度：
  ✅ 代码构建完成
  ✅ 单元测试通过（95% 覆盖率）
  ✅ 容器镜像构建完成
  ✅ 部署到 AKS DEV
  ✅ 健康检查通过
🎉 部署成功！
🔗 API 地址：http://api-dev.mdm.com/products
📊 监控面板：http://monitor.mdm.com/dashboard
```

## Vibe Coding 工作流

### 完整流程示例

**场景：** PO 想要快速验证一个新的产品查询 API

**Step 1: 需求定义（5分钟）**
```
PO → MDM Agent Web UI

描述需求：
"我需要一个 API，根据品牌和国家查询产品列表，
返回产品名称、GTIN、价格、库存状态"
```

**Step 2: Agent 生成方案（2分钟）**
```
Agent 分析：
✅ 识别数据表：Product, Brand, Region
✅ 生成 SQL 查询
✅ 设计 API 接口
✅ 生成 API 代码
✅ 生成 Swagger 文档
```

**Step 3: PO 预览和调整（3分钟）**
```
预览：
- SQL 查询语句
- API 接口定义
- 示例请求/响应

调整：
- 添加分页参数
- 添加排序功能
```

**Step 4: 一键部署（5分钟）**
```
Agent 自动执行：
✅ 生成完整代码
✅ 运行单元测试
✅ 构建 Docker 镜像
✅ 部署到 AKS DEV
✅ 注册到 API Gateway
```

**Step 5: 验证和分享（5分钟）**
```
验证：
- 测试 API 调用
- 查看监控数据
- 检查响应时间

分享：
- 生成 API 文档链接
- 分享给业务团队试用
```

**总耗时：20 分钟**（传统方式需要 2-4 周）

## 技术实现

### Agent 技术栈

| 组件 | 技术 | 用途 |
|------|------|------|
| **Agent Engine** | LangChain + Azure OpenAI | Agent 编排和推理 |
| **NLP** | GPT-4 | 自然语言理解 |
| **Code Generation** | GitHub Copilot API | 代码生成 |
| **SQL Generation** | Text-to-SQL Model | SQL 查询生成 |
| **CICD** | Azure DevOps API | 自动化部署 |
| **API Gateway** | Azure API Management | API 管理 |
| **Frontend** | React + Low-code UI | PO 操作界面 |

### Agent 编排

**LangChain Agent 流程：**
```python
from langchain.agents import Agent, Tool
from langchain.llms import AzureOpenAI

# 定义 Agent 工具
tools = [
    Tool(name="QueryData", func=query_data_tool),
    Tool(name="TransformData", func=transform_data_tool),
    Tool(name="GenerateAPI", func=generate_api_tool),
    Tool(name="DeployCICD", func=deploy_cicd_tool),
]

# 创建 Agent
agent = Agent(
    llm=AzureOpenAI(model="gpt-4"),
    tools=tools,
    prompt=mdm_agent_prompt
)

# PO 输入
po_request = "创建一个按品牌查询产品的 API"

# Agent 执行
result = agent.run(po_request)
```

### Web UI 设计

**Low-code 界面：**
- 🎯 **任务向导** - 引导式配置
- 📋 **模板库** - 常用场景模板
- 🔍 **预览模式** - 实时预览生成结果
- 🚀 **一键部署** - 快速发布
- 📊 **监控面板** - 实时监控

## 核心价值

### 对 PO 的价值

**提升效率：**
- 从 2-4 周 → 20 分钟
- 效率提升 100x+

**降低门槛：**
- 不需要写代码
- 不需要了解 SQL/API 开发
- 专注业务价值

**快速验证：**
- 快速试错
- 及时调整
- 降低失败成本

### 对团队的价值

**释放开发资源：**
- 减少重复性开发工作
- 开发团队专注复杂功能
- 提升整体产出

**标准化：**
- 统一代码风格
- 统一 API 规范
- 统一部署流程

**知识沉淀：**
- 最佳实践模板化
- 降低新人培训成本

## 项目计划

### Phase 1: MVP（Q3 2026）

**核心功能：**
- [ ] Data Query Agent（自然语言转 SQL）
- [ ] API Generator Agent（自动生成 CRUD API）
- [ ] CICD Agent（一键部署到 DEV）
- [ ] Web UI（基础界面）

**试点场景：**
- 产品查询 API 生成
- 简单数据转换

**预期结果：**
- 3 个 API 原型成功生成和部署
- PO 满意度 ≥ 4/5

### Phase 2: 功能扩展（Q4 2026）

**扩展功能：**
- [ ] Data Transform Agent（数据转换）
- [ ] Data Quality Agent（数据质量检查）
- [ ] 模板库（常用场景）
- [ ] 监控和告警

### Phase 3: 生产化（2027 Q1）

**生产就绪：**
- [ ] 安全加固（权限管理、审计）
- [ ] 性能优化
- [ ] 支持 SIT/UAT/PROD 部署
- [ ] 完整文档和培训

## 关键指标

| 指标 | 目标 | 说明 |
|------|------|------|
| **原型开发时间** | ≤ 30 分钟 | 从需求到可用原型 |
| **部署成功率** | ≥ 95% | 一键部署成功率 |
| **PO 自助率** | ≥ 60% | PO 独立完成的比例 |
| **代码质量** | ≥ B 级 | SonarQube 评分 |
| **开发资源节省** | 0.5 FTE | 减少重复开发工作 |

## 安全考虑

**安全机制：**
- **SQL 注入防护** - SQL 安全验证
- **权限控制** - 基于角色的访问控制
- **审计日志** - 所有操作可追溯
- **代码审查** - Agent 生成代码自动审查
- **环境隔离** - DEV/SIT/PROD 严格隔离

**风险缓解：**
- PO 只能部署到 DEV/SIT 环境
- PROD 部署需要 Tech Lead 审批
- 敏感数据访问需要额外授权
- 定期安全扫描

## 成功案例（规划）

**案例 1: 品牌产品查询 API**
- 需求：Marketing 团队需要按品牌查询产品
- 传统方式：2 周开发 + 1 周测试
- MDM Agent：20 分钟完成并部署到 DEV
- 结果：提前 3 周上线，快速响应业务需求

**案例 2: 数据质量报告**
- 需求：业务要临时查看产品数据质量
- 传统方式：等待 BI 团队排期（1-2 周）
- MDM Agent：5 分钟完成查询和报告
- 结果：即时决策，提升业务效率

## 技能要求

**PO 需要：**
- 基础的数据概念（表、字段）
- 清晰的业务需求描述
- 不需要编程技能 ✅

**团队需要：**
- LangChain Agent 开发
- Azure OpenAI API 使用
- Code Generation 技术
- DevOps 自动化

---

**文档版本:** v1.0  
**创建日期:** 2026-07-07  
**最后更新:** 2026-07-07
