# Service Operation (GDSN)

## 项目信息

| 项目属性 | 详情 |
|---------|------|
| **项目名称** | GDSN Service Operation |
| **适用系统** | GDSN (Global Data Synchronization Network) |
| **项目类型** | 🔄 Ongoing（持续运营） |
| **当前状态** | 进行中 |
| **优先级** | 🔴 High |

## 项目背景

GDSN 系统的日常运维工作，确保产品数据与全球零售商的同步服务稳定运行，包括数据发布、订阅管理、错误处理和合规性保障。

**核心职责：**
- GDSN 数据发布和同步监控
- 零售商数据订阅管理
- 数据质量和合规性保障
- 故障快速响应和解决

## GDSN 系统简介

### 什么是 GDSN?

**GDSN (Global Data Synchronization Network)** 是全球产品数据同步网络，用于在品牌商和零售商之间自动同步产品信息。

**核心概念:**
- **Data Pool** - 数据池（如 1WorldSync, GS1）
- **GTIN (Global Trade Item Number)** - 全球贸易项目代码
- **GLN (Global Location Number)** - 全球位置码
- **CIN (Catalogue Item Notification)** - 产品目录通知
- **Target Market** - 目标市场（零售商）

### GDSN 工作流程

```
产品数据准备 → 数据验证 → CIN 发布 → Data Pool 接收 → 零售商订阅 → 数据同步 → 确认/拒绝
```

## 主要工作内容

### 1. GDSN 数据发布管理

#### 数据发布流程

**Step 1: 数据准备**
- 从 PIM 系统提取产品数据
- GTIN 数据完整性检查
- 必填字段验证
- 图片和文档准备

**Step 2: 数据验证**
- **GS1 标准验证** - 符合 GS1 规范
- **零售商要求验证** - 特定零售商字段要求
- **数据质量检查** - 数据完整性和准确性

**Step 3: CIN 发布**
- 生成 CIN XML
- 发送到 Data Pool
- 接收发布确认
- 状态更新

**Step 4: 监控同步状态**
- 订阅状态监控
- 零售商接受/拒绝状态
- 错误处理

#### 发布类型

| 类型 | 说明 | 触发条件 |
|------|------|---------|
| **Initial Publication** | 首次发布 | 新产品 |
| **Update** | 数据更新 | 产品信息变更 |
| **Correction** | 数据修正 | 零售商拒绝后修正 |
| **Discontinuation** | 产品下架 | 产品停产 |

#### 数据发布指标

| 指标 | 目标 | 说明 |
|------|------|------|
| **发布成功率** | ≥ 95% | 首次发布成功率 |
| **Data Pool 响应时间** | ≤ 2 h | 发布后确认时间 |
| **零售商接受率** | ≥ 85% | 零售商首次接受率 |
| **数据完整性** | 100% | 必填字段完整 |

### 2. 零售商数据订阅管理

#### 零售商订阅配置

**主要零售商:**
- **Walmart** - 沃尔玛（全球）
- **Carrefour** - 家乐福（欧洲）
- **Tesco** - 乐购（英国）
- **Target** - Target（美国）
- **REWE** - REWE（德国）

**订阅配置:**
- GLN 配置（零售商 GLN + 我方 GLN）
- Target Market 设置
- 数据模板配置（不同零售商字段要求不同）
- 自动订阅规则

#### 订阅状态监控

**订阅生命周期:**
1. **Pending** - 等待零售商确认
2. **Accepted** - 零售商接受
3. **Synchronised** - 数据同步成功
4. **Rejected** - 零售商拒绝（需修正）
5. **Withdrawn** - 产品撤回

**监控任务:**
- 每日订阅状态检查
- 拒绝原因分析
- 修正和重新发布
- 零售商沟通协调

#### 订阅问题处理

**常见拒绝原因:**
- 必填字段缺失
- 数据格式不符合要求
- GTIN 校验失败
- 图片质量不合格
- 产品分类错误

**处理流程:**
1. 识别拒绝原因
2. 从 Data Pool 获取详细错误
3. 数据修正（PIM 或 MDM）
4. 重新发布 CIN
5. 验证接受状态

### 3. GDSN 数据质量管理

#### 数据质量检查

**GS1 标准合规:**
- GTIN 校验（Check Digit）
- 产品分类（GPC - Global Product Classification）
- 计量单位标准（UN/CEFACT）
- 国家代码（ISO 3166）

**零售商特殊要求:**
- Walmart - 图片分辨率、营养成分
- Carrefour - 多语言描述、欧盟合规标识
- Target - 成分、过敏原信息
- Tesco - 包装可回收性信息

#### 数据增强

**图片管理:**
- 主图（Front view, 高分辨率 ≥ 1000x1000px）
- 多角度图片（Side, Back, Top）
- 产品使用图片
- 图片格式（JPEG, PNG）
- 图片大小限制（通常 ≤ 5MB）

**多语言支持:**
- 产品描述翻译
- 成分和营养成分翻译
- 目标市场语言配置

### 4. GDSN 系统维护

#### 系统组件

**数据集成:**
- **PIM → GDSN 数据同步** - 每日批量 + 实时触发
- **MDM → GDSN 主数据同步** - GTIN, GLN
- **GDSN → Data Pool 连接** - API/SFTP

**Data Pool 连接:**
- **1WorldSync** - 主要 Data Pool
- **GS1 Net** - 备用 Data Pool
- 连接配置和认证管理
- 连接监控和故障切换

#### 系统监控

**关键监控指标:**

| 监控项 | 告警阈值 | 处理 |
|--------|---------|------|
| **CIN 发布失败** | > 5% | 检查 Data Pool 连接 |
| **订阅拒绝率** | > 20% | 数据质量问题排查 |
| **同步延迟** | > 4 h | Data Pool 性能检查 |
| **API 错误率** | > 1% | 连接和认证检查 |

**日志和审计:**
- CIN 发布日志
- 订阅状态变更日志
- 数据修正历史
- 零售商通信记录

#### 定期维护任务

**每日任务:**
- [ ] 检查 CIN 发布状态
- [ ] 检查订阅拒绝情况
- [ ] 处理数据修正请求
- [ ] 监控系统告警

**每周任务:**
- [ ] 生成订阅状态报告
- [ ] 分析拒绝原因趋势
- [ ] 零售商数据质量审查
- [ ] Data Pool 连接测试

**每月任务:**
- [ ] 月度数据质量报告
- [ ] 零售商满意度评估
- [ ] 系统性能分析
- [ ] 合规性审计

### 5. 零售商关系管理

#### 零售商对接

**沟通渠道:**
- Data Pool 支持团队
- 零售商 GDSN 团队
- 供应商门户（Vendor Portal）
- 邮件和 Ticket 系统

**定期沟通:**
- 月度数据质量回顾
- 新产品发布计划沟通
- 数据规范变更通知
- 问题协调和解决

#### 零售商需求管理

**需求类型:**
- 新增数据字段要求
- 数据格式变更
- 图片规格变更
- 合规性要求更新

**处理流程:**
1. 需求收集和确认
2. 影响评估（PIM, MDM, GDSN）
3. 实施方案设计
4. 开发和测试
5. 零售商验证
6. 正式上线

## GDSN 运维流程

### 日常运维 SOP

**上午任务:**
1. 检查 CIN 发布队列
2. 检查夜间批量发布结果
3. 检查订阅状态变更
4. 处理高优先级拒绝

**下午任务:**
1. 数据修正和重新发布
2. 零售商 Ticket 响应
3. 数据质量问题调查
4. 系统监控和告警处理

### 问题升级流程

**L1 - 日常运维（GDSN Operator）**
- 标准数据修正
- 常见拒绝处理
- 系统监控

**L2 - 技术支持（GDSN Specialist）**
- 复杂数据问题
- Data Pool 对接
- 系统故障排查

**L3 - 专家团队（GDSN Architect + Data Pool Vendor）**
- 架构问题
- Data Pool 底层问题
- 重大合规性问题

## 关键指标

| 指标 | 目标 | 说明 |
|------|------|------|
| **CIN 发布成功率** | ≥ 95% | 首次发布成功 |
| **零售商接受率** | ≥ 85% | 首次接受率 |
| **数据同步时效** | ≤ 4 h | 从发布到同步 |
| **拒绝解决时间** | ≤ 2 天 | 从拒绝到修正重发 |
| **数据质量合规** | 100% | GS1 标准合规 |
| **系统可用性** | ≥ 99.5% | GDSN 服务可用性 |

## GDSN 技术架构

### 系统组件

```
PIM (Product Information) → GDSN Adapter → Data Pool (1WorldSync) → Retailers
                          ↓
                     MDM (Master Data)
```

**GDSN Adapter:**
- CIN Generator
- Validation Engine
- Subscription Monitor
- Error Handler

**数据库:**
- GDSN Staging DB - 待发布数据
- GDSN History DB - 发布历史
- Subscription Status DB - 订阅状态

**集成接口:**
- PIM API - 产品数据获取
- MDM API - GTIN/GLN 验证
- Data Pool API - CIN 发布
- Data Pool SFTP - 批量数据交换

## 工具和文档

### 使用工具
- **1WorldSync Portal** - Data Pool 管理
- **GS1 Validator** - 数据验证
- **GDSN Adapter (Internal)** - 自研适配器
- **Azure Data Factory** - 数据集成
- **Power BI** - 订阅状态报表

### 文档和手册
- GDSN Operations Runbook
- 零售商数据要求清单
- GS1 标准规范
- Troubleshooting Guide
- 零售商对接手册

## GDSN 运维团队

| 角色 | 职责 | 人员 |
|------|------|------|
| **GDSN Lead** | 整体运维管理 | 待定 |
| **GDSN Operator** | 日常运维 | 待定 |
| **Data Quality Analyst** | 数据质量 | 待定 |
| **Retailer Liaison** | 零售商对接 | 待定 |

---

**文档版本:** v1.0  
**创建日期:** 2026-07-07  
**最后更新:** 2026-07-07
