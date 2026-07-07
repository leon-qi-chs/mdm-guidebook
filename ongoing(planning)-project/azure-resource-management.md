# Azure Resource Management

## 项目信息

| 项目属性 | 详情 |
|---------|------|
| **项目名称** | Azure Resource Management |
| **适用范围** | 所有 Azure 云资源 |
| **项目类型** | 🔄 Ongoing（持续运营） |
| **当前状态** | 进行中 |
| **优先级** | 🔴 High |

## 项目背景

Azure 云资源的整体管理工作，包括资源规划、成本优化、权限管理、安全合规和资源监控，确保云资源的高效利用和安全性。

**核心职责：**
- Azure 资源生命周期管理
- 成本监控和优化
- 权限和安全管理
- 合规性保障

## 主要工作内容

### 1. Azure 资源生命周期管理

#### 资源规划

**容量规划:**
- 计算资源（VM, AKS, App Service）
- 存储资源（Storage Account, Disk）
- 数据库资源（SQL Database, Cosmos DB）
- 网络资源（VNet, Load Balancer）

**资源申请流程:**
1. 提交资源申请（Jira/ServiceNow）
2. 成本评估
3. 审批（Manager 或 FinOps）
4. 资源创建（IaC - Terraform/Bicep）
5. 配置和交付
6. 文档更新

#### 资源创建和配置

**基础设施即代码 (IaC):**
- **Terraform** - 多云资源管理
- **Bicep** - Azure 原生 IaC
- **ARM Templates** - 传统 Azure 模板

**资源命名规范:**
```
{resource-type}-{project}-{env}-{region}-{sequence}

示例:
- aks-mdm-prod-eastus-001
- sql-pim-dev-eastus-001
- st-goldenpipe-prod-eastus-001
```

**资源标签（Tags）规范:**
- **Project**: 项目名称
- **Environment**: dev/sit/uat/prod
- **Owner**: 负责人邮箱
- **CostCenter**: 成本中心
- **CreatedDate**: 创建日期
- **ExpiryDate**: 到期日期（临时资源）

#### 资源变更管理

**变更流程:**
- 变更申请（Change Request）
- 影响分析
- 变更实施（通过 IaC）
- 验证和测试
- 文档更新

**变更类型:**
- 资源配置变更（Scale Up/Down）
- 资源迁移
- 资源删除
- 安全策略变更

#### 资源下线管理

**资源回收流程:**
1. 确认资源不再使用
2. 数据备份和迁移（如需要）
3. 通知相关团队
4. 资源删除
5. 成本核销
6. 文档更新

### 2. 成本监控和优化

#### 成本监控

**监控维度:**
- 按资源类型
- 按项目（Tag: Project）
- 按环境（Tag: Environment）
- 按成本中心（Tag: CostCenter）

**成本预警:**
- 月度预算超支告警（80%, 100%, 120%）
- 异常成本增长告警
- 未标记资源告警
- Idle 资源告警

#### 成本优化策略

**计算资源优化:**
- **Reserved Instances (RI)** - 1 年/3 年折扣
- **Azure Hybrid Benefit** - Windows Server 许可
- **Spot Instances** - 非关键工作负载
- **Right-sizing** - 资源规格调优
- **Auto-shutdown** - DEV/SIT 环境自动关机

**存储优化:**
- **Storage Tier** - Hot/Cool/Archive 层级选择
- **Lifecycle Management** - 自动归档策略
- **数据压缩** - 减少存储空间
- **Orphaned Disk 清理** - 删除未使用磁盘

**数据库优化:**
- **Elastic Pool** - 共享资源池
- **Serverless** - 按使用付费
- **Read Replica** - 就近访问
- **Backup 策略优化** - LTR 成本控制

**网络优化:**
- **VNet Peering** - 减少数据传输
- **CDN** - 内容分发网络
- **ExpressRoute** - 专线连接（大流量）

#### 成本归因和报告

**月度成本报告:**
- 总体成本趋势
- 各项目成本明细
- Top 10 高成本资源
- 成本异常分析
- 优化建议

**成本分摊:**
- 共享资源成本分摊算法
- 项目成本分配
- Chargeback 报告

### 3. 权限和安全管理

#### Azure RBAC (Role-Based Access Control)

**角色定义:**

| 角色 | 权限范围 | 适用人员 |
|------|---------|---------|
| **Owner** | 完全控制 | Cloud Admin |
| **Contributor** | 创建和管理资源 | DevOps, SRE |
| **Reader** | 只读访问 | Developer, Analyst |
| **Custom Role** | 定制权限 | 按需定义 |

**权限申请流程:**
1. 提交权限申请（Jira）
2. Manager 审批
3. 最小权限原则评估
4. 权限授予（Azure Portal/CLI）
5. 定期权限审查（每季度）

#### Azure AD 集成

- **Single Sign-On (SSO)** - 统一登录
- **Multi-Factor Authentication (MFA)** - 双因素认证
- **Conditional Access** - 条件访问策略
- **Privileged Identity Management (PIM)** - 特权访问管理

#### 安全策略

**Azure Policy:**
- 强制资源标签
- 限制资源创建区域
- 强制加密（Storage, Database）
- 禁止公网访问（除白名单）
- 强制备份策略

**Network Security:**
- **NSG (Network Security Group)** - 网络访问控制
- **Azure Firewall** - 中心化防火墙
- **Private Endpoint** - 私有连接
- **Service Endpoint** - 服务端点

**数据加密:**
- **Encryption at Rest** - 静态数据加密
- **Encryption in Transit** - 传输加密 (TLS 1.2+)
- **Key Management** - Azure Key Vault

### 4. 合规性和审计

#### 合规要求

**数据驻留:**
- 数据存储位置（中国区/海外）
- 跨境数据传输合规

**行业标准:**
- **ISO 27001** - 信息安全管理
- **SOC 2** - 服务组织控制
- **GDPR** - 欧盟数据保护

#### 审计日志

**Azure Activity Log:**
- 资源创建/删除/修改记录
- 权限变更记录
- 配置变更记录

**Log Analytics:**
- 集中审计日志
- 异常行为检测
- 合规性报告

**定期审计:**
- 月度资源审计（未使用资源）
- 季度权限审计（权限回收）
- 年度合规审计

### 5. 灾难恢复 (DR)

#### 备份策略

**备份对象:**
- 数据库（每日全量 + 增量）
- 存储账户（重要数据）
- VM 磁盘（关键系统）
- 配置文件（IaC 代码）

**备份保留:**
- 日备份：30 天
- 周备份：12 周
- 月备份：12 月
- 年备份：7 年（合规要求）

#### 灾难恢复计划

**RTO/RPO 目标:**

| 系统 | RTO | RPO | DR 策略 |
|------|-----|-----|---------|
| **Golden PIPE (PROD)** | 4 h | 15 min | Geo-Redundant + Backup |
| **PIM (PROD)** | 4 h | 15 min | Geo-Redundant + Backup |
| **MDM API (PROD)** | 2 h | 5 min | Multi-Region Active-Active |
| **Non-PROD** | 24 h | 24 h | Backup only |

**DR 演练:**
- 频率：每季度一次
- 范围：关键生产系统
- 验证：恢复时间、数据完整性
- 改进：更新 DR 文档

## Azure 资源管理流程

### 资源申请和审批

**申请表单:**
- 资源类型和规格
- 预估成本
- 业务理由
- 使用期限

**审批矩阵:**

| 月成本 | 审批人 |
|--------|--------|
| < $500 | Tech Lead |
| $500 - $2000 | Manager |
| > $2000 | Manager + FinOps |

### 成本审查会议

**频率**: 每月一次

**参与者**:
- Cloud Admin
- FinOps Lead
- Project Owner
- Manager

**议程**:
1. 上月成本回顾
2. 成本异常分析
3. 优化建议讨论
4. 下月预算规划

## 关键指标

| 指标 | 目标 | 说明 |
|------|------|------|
| **资源标签覆盖率** | ≥ 95% | 必填标签完整性 |
| **成本节省** | ≥ 15% YoY | 年度成本优化 |
| **预算偏差** | ≤ 5% | 月度预算准确性 |
| **未使用资源** | 0 | Idle 资源及时清理 |
| **安全策略合规** | 100% | Azure Policy 合规 |
| **备份成功率** | ≥ 99% | 备份作业成功率 |

## MDM 资源清单

### 当前资源

**计算资源:**
- **AKS Cluster** - mdm-prod-eastus (3 Node Pool)
- **App Service** - PIM APIs, Golden PIPE APIs
- **Function App** - ETL Serverless Functions

**数据库:**
- **SQL Database** - MDM Master DB, PIM DB
- **Cosmos DB** - Product Catalog (Global)
- **PostgreSQL** - Analytics DB

**存储:**
- **Storage Account** - Data Lake (Delta Tables)
- **Blob Storage** - 文件存储，备份
- **File Share** - 共享文件

**网络:**
- **VNet** - MDM VNet (10.0.0.0/16)
- **Load Balancer** - API Gateway
- **Private Endpoint** - Database 连接

**数据集成:**
- **Data Factory** - ETL Pipelines
- **Event Hub** - Real-time Streaming
- **Service Bus** - Message Queue

### 月度成本估算

| 资源分类 | 月成本 (USD) | 占比 |
|---------|------------|------|
| **计算** | $5,000 | 35% |
| **数据库** | $4,000 | 28% |
| **存储** | $2,000 | 14% |
| **数据集成** | $2,000 | 14% |
| **网络** | $1,000 | 7% |
| **其他** | $300 | 2% |
| **总计** | **$14,300** | 100% |

## 工具和平台

### 使用工具
- **Azure Portal** - 资源管理界面
- **Azure CLI** - 命令行管理
- **Terraform** - IaC 工具
- **Azure Cost Management** - 成本分析
- **Azure Policy** - 策略管理
- **Azure Advisor** - 优化建议
- **Azure Monitor** - 监控和告警

### 文档和清单
- 资源清单（Resource Inventory）
- 成本分析报告
- 权限矩阵
- DR 计划
- IaC 代码仓库

## Azure Resource Management 团队

| 角色 | 职责 | 人员 |
|------|------|------|
| **Cloud Admin** | 资源全生命周期管理 | 待定 |
| **FinOps Lead** | 成本优化 | 待定 |
| **Security Engineer** | 安全合规 | 待定 |
| **Backup Admin** | 备份和恢复 | 待定 |

---

**文档版本:** v1.0  
**创建日期:** 2026-07-07  
**最后更新:** 2026-07-07
