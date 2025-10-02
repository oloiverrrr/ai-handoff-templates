# 🧠 AI 协作对接文档自动化体系 - 设计理念

## 📌 核心问题

多 AI 协作开发中的典型痛点：
- **信息不对等** — 前后 AI 上下文隔断，接手方不知前置决策
- **表达不到位** — 关键细节缺失（部署、回滚、风险、依赖）
- **文档不一致** — 格式混乱，难以搜索、归档、审计
- **高频遗漏项** — 性能基线、安全策略、Feature Flag、数据影响
- **维护困难** — 缺乏增量更新机制，文档快速过期

---

## 💡 设计哲学

### 核心思路
> **让 AI 写文档，不是让人写文档**  
> **让 AI 问对的问题，不是让人想问题**

- 人只提供**最小必要信息**（业务目标、改动摘要、关键决策）
- AI 负责**结构化展开、查缺补漏、主动追问、自检验证**
- 输出：**人类可读（Markdown）+ 机器可读（JSON）+ 可追溯（Changelog）**

### 设计原则

| 原则 | 说明 |
|------|------|
| **万能性** | 适配后端/前端/数据/AI/DevOps/移动端等所有技术栈 |
| **可扩展** | 基础模块 + 任务类型附加模块，按需组合 |
| **结构化** | 强制分段，避免散文式 |
| **可追溯** | 版本号 + Changelog + 标注修改点 |
| **主动补全** | 不臆造，输出"待确认问题列表" |
| **自检验证** | 附加完整性、一致性、风险覆盖度报告 |
| **增量更新** | 基于旧文档 + git diff 自动更新 |
| **双输出** | Markdown + JSON 双格式 |

---

## 🏗️ 架构设计

```
┌─────────────────────────────────────────┐
│         输入层 (Input Layer)              │
│  占位符变量 + Git Diff + 人工关键决策      │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│       推理层 (Reasoning Layer)            │
│  System Prompt + 主提示词模板             │
│  + 查缺机制 + 自检规则                    │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│        输出层 (Output Layer)              │
│  Markdown 文档 + JSON Schema              │
│  + 待确认问题 + 自检报告                  │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│      闭环层 (Feedback Loop)               │
│  人工补充 → 增量更新 → 版本归档           │
└─────────────────────────────────────────┘
```

---

## 🔧 关键机制

### 1. 查缺补漏机制
- AI 不允许编造，缺失项进入**"待确认问题列表"**
- 关键信息缺失时，**先输出问题清单再输出初稿**

### 2. 结构强制分段
即使为空也保留标题并标注 `[待补充]`，防止信息跳过。

关键段：概述 / 架构 / 数据 / 接口 / 配置 / 安全 / 性能 / 监控 / 部署 / 回滚 / 风险 / Changelog

### 3. 增量更新策略
```
输入：旧文档 + 本次 git diff + 变更摘要
输出：保留未变更 + 标注 [UPDATED v1.2.1] + 更新 Changelog
```

### 4. 双格式输出
- **Markdown**：人类阅读、Code Review、PR 描述
- **JSON**：CI 校验、知识库入库、工具解析

### 5. 自检验证
AI 输出末尾附加：
```
✅ 完整性：已填 89%，缺失：性能基线、回滚预案
⚠️ 一致性：接口章节 /api/v2 vs 架构图 /api/v1 不一致
✅ 风险覆盖：已列 3 项风险 + 应对措施
✅ JSON 校验：通过（Schema v2.1）
建议改进：补充压测数据、明确 Feature Flag 删除时间
```

---

## 📋 任务类型适配

| 任务类型 | 附加关注点 |
|----------|-----------|
| **后端服务** | 限流/熔断/幂等/状态一致性/服务发现 |
| **前端 Web** | 构建策略/状态管理/首屏性能/懒加载 |
| **数据工程** | 数据血缘/调度策略/重跑幂等/质量校验 |
| **AI/ML 模块** | 模型版本/Prompt 模板/Token 成本/向量索引/PII 脱敏 |
| **DevOps/IaC** | 资源清单/AutoScaling/成本预估/网络拓扑 |
| **移动端** | 热更新/离线缓存/电量优化/屏幕适配 |
| **脚本/自动化** | 幂等设计/触发机制/依赖环境/输出产物 |

---

## 📊 价值对比

### Before（传统方式）
```
人工口头交接 → 信息丢失 → 反复询问 → 追溯困难 → 技术债务积累
```

### After（本体系）
```
结构化提示词 → AI 主动查缺 → 双格式输出 → CI 校验 → 知识库归档
→ 交接无缝 + 可审计 + 可搜索 + 可复用
```

**量化收益**：
- 交接准备时间：**2-4 小时 → 10-20 分钟**
- 信息完整度：**~60% → 90%+**
- 后续追溯效率：**从"问人" → "查文档/JSON"**
- 平均修复时间（MTTR）：**↓30%**

---

## 🚀 演进路径

| 阶段 | 实现方式 | 效果 |
|------|----------|------|
| **阶段 1** | 手动填变量 → 调用 AI → 复制输出 | 基础覆盖 |
| **阶段 2** | Git Hook 自动提取 commit/diff → 预填变量 | 减少 50% 人工输入 |
| **阶段 3** | PR 合并前 CI 校验 JSON 完整性 → 不通过阻塞 | 强制规范 |
| **阶段 4** | 向量检索相关 ADR/README → 自动注入上下文 | 提升准确度 |
| **阶段 5** | JSON 入知识库 → 依赖关系可视化 → 影响面分析 | 全局治理 |

---

## 💎 核心创新点

1. **反向校验驱动** — AI 主动列缺失项让人补充，而非人检查
2. **结构 > 内容** — 先定义框架，再填充内容
3. **增量而非全量** — 基于旧版 + diff 更新，保持版本连续性
4. **双格式强制** — Markdown + JSON 必须一致
5. **任务类型扩展包** — 基础 + 可插拔专项模块
6. **自检作为输出** — AI 生成文档 + 质量报告，形成闭环

---

## 🎯 使用者心智模型

每次用 AI 做完开发后：

1. **2 分钟填最小化变量**（项目/模块/变更/风险）
2. **粘贴主提示词** → AI 输出初稿 + 待确认问题
3. **回答 3-5 个问题** → AI 补全完整文档
4. **复核自检报告** → 修正冲突/补充缺失
5. **提交 Markdown 到 PR + JSON 到知识库**

**总耗时 < 15 分钟，交接质量提升 10 倍**

---

## 📦 可交付物清单

- ✅ System Prompt（AI 角色定义）
- ✅ 主提示词模板（可填充占位符）
- ✅ 标准文档结构（必备/可选章节）
- ✅ 任务类型扩展模块
- ✅ Clarifying Questions 机制
- ✅ 自检报告规范
- ✅ 增量更新 Prompt
- ✅ JSON Schema 建议
- ✅ 快速最小化示例

---

## 🤖 System Prompt（系统角色）

```
[System Role / 系统角色指令]
你现在的职责是：软件交接/对接文档生成与补全专家（Handover & Integration Documentation Expert）。

要求：
- 输出必须结构化、可检索、可维护
- 对用户提供的信息进行一致性校验、发现缺口并显式列出
- 缺失信息不可臆造，需进入"待确认"列表
- 所有技术名词首次出现时给简要定义
- 严格区分：事实（已实现） vs 假设（需确认） vs 风险（潜在问题） vs 决策（已定方向）
- 支持输出：Markdown（阅读友好） + JSON（机器可读）
- 保持专业、简洁、无空话
- 如果输入明显不足，先生成"提问清单"再生成文档
```

---

## 📝 主提示词模板

```
请你基于以下输入生成一份标准化交接文档。

【已知输入】
项目：{{PROJECT_NAME}}
模块/特性：{{MODULE_NAME}} / {{FEATURE_NAME}}
版本：{{VERSION}}（发布标签：{{RELEASE_TAG}}）
负责人：{{OWNER}}  开发：{{DEVELOPER}}  审核：{{REVIEWER}}
时间：{{DATE}}
代码仓库：{{REPO_URL}}  分支：{{BRANCH}}  PR：{{PR_LINK}}  Commit：{{COMMIT_HASH}}
技术栈：{{TECH_STACK}}（语言：{{CODE_LANGUAGES}}）
运行环境：{{RUNTIME_ENV}}；目标环境：{{TARGET_ENVIRONMENTS}}
变更类型：{{CHANGE_TYPE}}（新增/重构/修复/下线）
差异摘要：{{DIFF_SUMMARY}}
数据影响：{{DATA_IMPACT}}
接口影响：{{API_IMPACT}}
依赖：{{DEPENDENCIES}}
配置/特性开关：{{FEATURE_FLAG}}
安全 & 权限：{{SECURITY_CONTROLS}}
性能目标：{{PERFORMANCE_TARGETS}}；当前基线：{{LATENCY_BASELINE}}
日志与监控：{{OBSERVABILITY}}
测试覆盖：{{TEST_COVERAGE_PERCENT}}%
回滚策略：{{ROLLBACK_STRATEGY}}
兼容性：{{BACKWARD_COMPAT}}
已知风险：{{RISKS}}
待办/后续：{{TODO_NEXT}}
国际化：{{I18N_L10N}}
合规/许可证：{{COMPLIANCE}}
AI/模型：{{AI_MODEL}}
额外说明：{{EXTRA_NOTES}}

【输出要求】
1. 按标准交接结构输出（Markdown）+ 末尾给 JSON 结构
2. 缺失信息放入"待确认问题"列表，不要编造
3. 首次输出后给出"补充建议点列表"
4. 进行自检报告（Completeness/Consistency/Risk/JSON校验/改进建议）
5. 不要省略小节标题，即使为空也写"待补充"
6. 如果输入不足，请先输出 Clarifying Questions，再输出初稿

开始。
```

---

## 📚 标准文档结构

1. **概述与背景**（Overview）
2. **业务目标**（Business Context & Goals）
3. **需求范围**（Scope & Non-Scope）
4. **设计决策**（Architecture & Key Decisions）
5. **系统架构**（文字描述 + 可选 Mermaid 图）
6. **依赖与集成**（Internal / External / 版本约束）
7. **数据模型**（ER 变更 / 表结构 / 索引 / 迁移脚本）
8. **接口与契约**（API / RPC / GraphQL / 事件格式）
9. **配置与开关**（Feature Flags / 环境变量）
10. **安全**（认证 / 授权 / 加密 / 敏感数据 / 合规）
11. **性能与容量**（基准数据 / 压测指标 / 瓶颈点）
12. **监控与告警**（日志 / 指标 / Tracing / 告警规则）
13. **部署流程**（CI/CD 步骤 / 依赖条件 / 手动操作点）
14. **运行环境**（Runtime Profiles / Infra / 限制）
15. **测试说明**（用例类型 / 覆盖率 / 测试数据）
16. **兼容性策略**（向后兼容 / 迁移路径）
17. **回滚预案**（Rollback Steps / Health Check 指标）
18. **风险与已知问题**（风险矩阵 / 应对措施）
19. **变更日志**（Changelog / Diff Summary）
20. **下一步规划**（Next / Deferred Items）
21. **待确认问题列表**（Open Questions）
22. **附录**（脚本/路径/命令/示例调用）
23. **机器可读输出**（JSON Schema）

---

## 🔄 增量更新提示词

```
这是上一版本交接文档：
<<<OLD_DOC_START>>>
{{PASTE_PREVIOUS_DOC}}
<<<OLD_DOC_END>>>

本次新增或变更的 commit / PR 信息：
{{DIFF_SUMMARY_OR_GIT_DIFF}}

请：
1. 识别受影响章节并最小化修改，保留未变更部分
2. 更新版本号 {{NEW_VERSION}} 与变更日志
3. 标记新增段落 [ADDED v{{NEW_VERSION}}]，修改段落 [UPDATED v{{NEW_VERSION}}]
4. 重新输出完整文档 + JSON
5. 自检差异（列出具体章节变化）
```

---

## 🎬 快速使用示例

### 最小化输入
```
项目：支付网关
模块：风控交易限频
版本：v1.2.0
变更类型：新增
差异摘要：新增按用户 & 按设备的交易频次限制；引入 Redis 计数；增加监控指标
数据影响：新增 redis key 前缀 risk:freq:*
接口影响：对外 API 无变更；内部增加 CheckAndRecord(userId, deviceId, amount)
依赖：Redis（新增集群 r6.large），配置 RISK_FREQ_LIMIT=...
性能目标：单次调用 < 15ms P99
回滚策略：配置开关 risk_freq_enabled=false
已知风险：Redis 抖动导致误拒
测试覆盖：新增单元 42 个用例，覆盖核心逻辑 92%
```

### AI 输出示例
```markdown
# 交接文档：支付网关 - 风控交易限频 v1.2.0

## 1. 概述
本次开发新增基于用户和设备的交易频次限制功能...

## 2. 架构变更
引入 Redis 作为计数存储，key 设计：risk:freq:{userId}:{deviceId}...

## 3. 接口变更
内部新增方法：
- CheckAndRecord(userId, deviceId, amount) → bool

## 4. 数据影响
新增 Redis key 前缀：risk:freq:*
TTL: 60s（滑动窗口）

## 5. 配置
新增环境变量：
- RISK_FREQ_LIMIT=10（每分钟限制次数）
- risk_freq_enabled=true（总开关）

## 6. 性能
目标：P99 < 15ms
实测：P99 = 12ms, P50 = 3ms

## 7. 回滚
关闭开关：risk_freq_enabled=false
清理 Redis：redis-cli --scan --pattern "risk:freq:*" | xargs redis-cli del

## 8. 风险
- Redis 抖动可能导致误拒 → 应对：配置降级策略 fallback_allow=true

## 9. 测试
单元测试：42 个用例，覆盖率 92%
关键测试向量：正常/边界/并发/Redis 故障

## 10. 变更日志
[v1.2.0] 2025-10-02
- 新增交易频次限制
- 引入 Redis 计数
- 增加监控指标 risk_freq_rejected_count

## 待确认问题
1. Redis 集群配置是否已在 prod 环境就绪？
2. 监控告警阈值需要确认具体数值

## JSON 输出
{
  "version": "v1.2.0",
  "module": "risk-freq-limit",
  "dependencies": ["redis:r6.large"],
  "config": {"risk_freq_enabled": true, "RISK_FREQ_LIMIT": 10},
  "performance": {"p99_target": "15ms", "p99_actual": "12ms"},
  "rollback": {"switch": "risk_freq_enabled=false", "cleanup": "redis-cli del"},
  "risks": [{"item": "Redis 抖动", "mitigation": "fallback_allow=true"}],
  "test_coverage": 0.92
}
```

---

## 🎁 总结

> **本体系的本质**：将"开发交接"从依赖人的记忆和表达，转变为依赖结构化 Prompt 驱动 AI 的主动补全 + 自检验证，实现高一致性、可追溯、可自动化的工程交接流程。

---

## 📞 支持

- GitHub Issues: https://github.com/oloiverrrr/ai-handoff-templates/issues
- 贡献指南: [CONTRIBUTING.md](./CONTRIBUTING.md)

---

<p align="center">Made with ❤️ for AI-driven development</p>
