# DevDocs Toolkit 详细工具箱方案（v1.0）

> 日期：2026-05-23  
> 状态：基线方案（用于主基调/架构/路线确认）


## 0. 文档定位与边界（定稿说明）

本文件是 **DevDocs Toolkit 的主基调与基线架构文档**，用于统一：
- 产品定位与范围边界；
- 工具箱能力结构与演进路线；
- 后续开发计划文档的编制依据。

### 明确边界
- 本文档 **不包含** 开发任务派单细节。
- 本文档 **不绑定** 特定AI厂商执行分工。
- 本文档 **不覆盖** 技术概要设计、技术详细设计作为工具箱目标产出。

---

## 1. 方案目标与定位

DevDocs Toolkit 是一个面向"软件开发前期文档"的 AI 工具箱，核心目标是：
- 将前期文档产出从"人工拼接"升级为"标准化生产线"；
- 让产品经理、项目经理在个人模式下快速生成高质量文档；
- 建立可治理、可追溯、可扩展的企业级文档生产底座。

### 1.1 目标用户
- 第一批用户：产品经理、项目经理。
- 典型场景：调研与需求分析、PRD、UI原型、立项方案、汇报材料、测试用例。

### 1.2 阶段目标（v1.0）
- 文档首稿效率提升 ≥ 50%。
- 文档结构完整性评分 ≥ 80/100。
- 审核轮次从平均 3.0 降低到 ≤ 2.0。

---

## 2. 工具箱总体架构

采用四层架构：

1. **体验层（Experience）**
   - 对话入口（自然语言）
   - 参数化入口（结构化表单）

2. **编排层（Orchestration）**
   - Planner Agent：任务识别与参数补全
   - Producer Agent：文档生成与组装
   - Reviewer Agent：质量审校与改进建议

3. **能力层（Skills）**
   - core-*：通用能力（表格、流程图、Markdown等）
   - domain-*：领域能力（需求/PRD/设计/测试）
   - qa-*：质量能力（完整性、一致性、风险评分）

4. **治理层（Governance）**
   - 模板版本治理
   - 术语库治理
   - 输出命名规范
   - 指标监控（效率、返工、采纳率）

---

## 3. 呈现形式与交付形态

### 3.1 工具箱呈现形式
- **核心形态：Skill 包**（兼容 Claude Code、Codex、OpenCode）。
- **辅助形态：命令入口**（统一 `tool-*` 命令）。
- **输出形态：标准目录结构**（便于归档和复盘）。

### 3.2 标准目录建议

```text
DevDocs-Toolkit/
├── skills/
│   ├── core-*.md
│   ├── demand-*.md
│   ├── prd-*.md
│   ├── design-*.md
│   ├── qa-*.md
│   └── tool-*.md
├── templates/
│   ├── 需求分析.template.json
│   ├── PRD.template.json
│   ├── 立项方案.template.json
│   └── 测试用例.template.json
├── schemas/
│   ├── v0.2-minimum.json
│   └── mapping/
├── kb/
│   └── terminology.yaml
├── install/
│   ├── claude.sh
│   ├── codex.sh
│   └── opencode.sh
└── outputs/
    └── <project>/
```

---

## 4. 工具清单（9个）

### 4.1 7个产出型工具
1. `tool-survey-assist`：调研提纲、访谈纪要、需求初稿。
2. `tool-demand-analysis`：需求去重、优先级排序、需求追踪矩阵（RTM）。
3. `tool-prd-generator`：PRD主文档 + 数据字典 + 权限矩阵 + 业务流程图。
4. `tool-ui-prototype`：线框图、交互流程图、低保真原型、Design Token。
5. `tool-project-setup`：项目立项方案、里程碑计划、工作量估算。
6. `tool-report-material`：汇报材料（PPT + Word）结构化生成。
7. `tool-test-case-gen`：测试用例生成（功能/场景/边界/异常）。

> 说明：上述 7 个产出工具是工具箱标准能力集合；当前应优先落地的文档范围为"前期需求分析 + PRD"，其余工具按7大产出能力逐步扩展。

### 4.2 2个治理型工具
8. `tool-template-governance`：模板版本管理、差异对比、升级迁移。
9. `tool-quality-gate`：完整性评分、一致性评分、风险评分。

---

## 5. Agent 方案（3个核心Agent）

1. **Planner Agent**
   - 输入：用户目标、项目背景。
   - 输出：执行计划、缺失信息问询、调用顺序。

2. **Producer Agent**
   - 输入：已补全参数 + 模板 + Schema。
   - 输出：目标文档初稿（PRD等）。

3. **Reviewer Agent**
   - 输入：文档初稿 + 质量规则。
   - 输出：评分报告、问题列表、修复建议。

### 5.1 状态流
- Draft -> Review -> Approved -> Archived

---

## 6. Skill 体系（27个目标，9个先行）

### 6.1 目标全量（27）
- 基础层 5：core-table、core-flowchart、core-markdown、core-list-sorter、core-toc。
- 领域层 15：demand/prd/design/project/report/test。
- 聚合层 7：tool-*。

### 6.2 v0.2先行（9个）
- core-markdown-gen
- core-table-builder
- core-flowchart-gen
- core-toc-generator
- prd-data-dict-gen
- prd-role-matrix
- prd-flowchart-gen
- prd-page-list-gen
- tool-prd-generator

---

## 7. 数据模型与模板映射

### 7.1 最小统一Schema（建议）
- `Project`
- `Requirement`
- `Feature`
- `RolePermission`
- `NFR`
- `ProjectPlan`
- `TestCase`

### 7.2 四模板映射
- 前期需求分析 -> Requirement/Feature
- PRD -> Requirement/RolePermission/NFR
- 立项方案 -> ProjectPlan/Milestone/Risk
- 测试用例 -> TestCase/Scenario/Boundary

---

## 8. 实施治理与协作机制

### 8.1 角色职责（组织视角）
- **产品负责人**：确认文档标准、优先级与验收口径。
- **方案架构负责人**：维护架构一致性、Skill边界与命名规范。
- **模板治理负责人**：维护4类模板版本与映射。
- **质量负责人**：维护质量规则、评分标准与评审结论。

### 8.2 协作规范
- 统一输入输出契约：所有工具与技能遵循 `schemas/v0.2-minimum.json`。
- 分支策略：功能分支开发，周节奏合并到 `integration/v0.2`。
- 命名规范：工具名、字段名、输出目录统一使用英文 snake_case。
- 周评审机制：每周固定进行一次端到端演示与问题回归。

### 8.3 里程碑评审门
- M1（模板与Schema完成）：4模板结构化 + 最小Schema通过审查。
- M2（PRD闭环完成）：需求分析 -> PRD -> 质量报告完整跑通。
- M3（业务扩展联动）：PRD -> 立项方案 -> 测试用例链路打通。
- M4（发布评审）：KPI达标后进入v0.2发布。

---

## 9. 8周实施计划（可执行）

### Week 1-2
- 模板抽取与标准化
- Schema最小集定义
- 术语库初始化

### Week 3-4
- 实现 `tool-demand-analysis -> tool-prd-generator`
- 接入 `qa-prd-completeness-check`

### Week 5-6
- 打通 PRD -> 立项方案 -> 测试用例
- 完成质量汇总 `tool-quality-gate`

### Week 7-8
- 三平台安装联调（Claude/Codex/OpenCode）
- 3个真实项目试点回放
- 发布 v0.2

---

## 10. KPI 与验收标准

- PRD首稿时间下降 ≥ 40%。
- 文档完整性评分 ≥ 80/100。
- 术语一致性问题 ≤ 5条/文档。
- 审核轮次 ≤ 2.0。
- 试点满意度 ≥ 4.2/5。

---

## 11. 风险与应对

1. 模板频繁变更：双周冻结窗口 + 模板管理员。
2. 字段冲突：统一Schema为唯一事实源。
3. 跨平台差异：仅在安装层适配，不改业务层接口。
4. 可解释性不足：输出证据链与可编辑建议。

---

## 12. 立即启动清单（本周）

1. 锁定4份docx模板最终版本。
2. 创建 `templates/`、`schemas/`、`skills/` 骨架。
3. 完成模板抽取与Schema初稿（含字段映射）。
4. 打通最小闭环：`tool-demand-analysis -> tool-prd-generator`。
5. 第3天进行首次集成演示。

---

## 13. 基线确认清单（供最终确认）

在进入"开发计划文档"之前，请按以下清单确认：

1. **范围确认**：仅覆盖 7 个产出工具 + 2 个治理工具。
2. **边界确认**：不纳入技术概要设计、技术详细设计。
3. **优先级确认**：第一阶段以"前期需求分析 + PRD"闭环为优先。
4. **架构确认**：采用四层架构 + 3 Agent 编排模式。
5. **验收确认**：沿用第10章 KPI 作为阶段验收基准。

> 以上5项确认后，再进入下一份《开发计划文档（实施版）》编制。
