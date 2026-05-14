# 团队 AI 驱动开发工作流

> 整合 Harness Engineering + Symphony + Trellis 三家之长
> 适用于：3-20 人技术团队，AI Agent 作为核心生产力

---

## 核心哲学

```
人类 = 掌舵者（需求、决策、审查、品味）
Agent = 执行者（编码、测试、文档、部署）
仓库 = 唯一真相源（所有知识必须版本化）
```

---

## 团队角色

| 角色 | 职责 | 工具 |
|------|------|------|
| **产品经理** | 写需求/PRD、验收功能 | Linear / GitHub Issues |
| **设计师** | 设计稿、交互规范、验收 UI | Figma + 截图验收 |
| **工程师（人类）** | 架构决策、代码审查、异常处理 | PR Review + AGENTS.md 维护 |
| **Agent（Codex/Claude）** | 编码、测试、文档、部署 | 自动执行 |

---

## 仓库结构（团队版）

```
project-root/
├── AGENTS.md              # 团队地图（< 100 行）
├── .trellis/              # Trellis 规范系统
│   ├── spec/              # 团队规范（自动注入 Agent 上下文）
│   │   ├── architecture.md
│   │   ├── api-guidelines.md
│   │   ├── ui-patterns.md
│   │   └── golden-rules.md    # 黄金原则（编码到 linter）
│   ├── tasks/             # 任务目录
│   │   ├── active/        # 进行中
│   │   ├── pending/       # 待处理
│   │   └── completed/     # 已完成
│   └── workspace/         # 工作日志（按开发者独立）
│       ├── alice.journal
│       └── bob.journal
├── docs/                  # 知识库（Harness 模式）
│   ├── architecture/      # 架构文档
│   ├── design/            # 设计规范
│   ├── workflows/         # 工作流文档
│   ├── quality/           # 质量评分
│   └── troubleshooting/   # 故障排查
├── src/                   # 源代码
├── tests/                 # 测试
├── tools/                 # 自定义工具
│   ├── linters/           # 结构检查、依赖检查
│   ├── verify.py          # 环境自检
│   ├── diagnose.py        # 问题诊断
│   └── quality_report.py  # 质量报告
└── Makefile               # 统一命令入口
```

---

## 5 阶段工作流

### Stage 1: 需求输入（人类主导）

**谁参与**: PM + 设计师 + 工程师（可选）

**做什么**:
1. PM 在 Linear/GitHub Issues 创建任务
2. 写清晰的需求描述（用户故事 + 验收标准）
3. 设计师附加 Figma 链接或截图
4. 工程师评估技术可行性（如需要）

**输出**:
- Linear Issue / GitHub Issue
- 关联的 PRD（复杂功能）

---

### Stage 2: 自动规划（Agent 执行）

**触发**: Agent 自动拉取未分配的开放任务

**做什么**:
1. Agent 读取任务描述
2. 分析代码库当前状态
3. 生成执行计划（Plan）
   - 拆解子任务
   - 识别依赖关系（DAG）
   - 确定技术方案
4. 写入 `.trellis/tasks/active/{task-id}/`
   - `prd.md` - 需求文档
   - `plan.md` - 执行计划
   - `implement.jsonl` - 实现步骤
   - `check.jsonl` - 验证清单

**人类介入点**:
- 审查计划（如需要）
- 确认技术方案

---

### Stage 3: 并行实现（Agent 执行）

**做什么**:
1. Agent 按 plan.md 执行
2. 自动注入相关 Spec 作为上下文
3. 编码 → 测试 → 文档
4. 遇到依赖阻塞时自动等待
5. 遇到超出范围的问题自动创建子任务

**约束**:
- 遵循 AGENTS.md 的分层规则
- 遵循 golden-rules.md（linter 强制执行）
- 所有代码必须有测试覆盖
- 使用结构化日志（禁止 print）

**Agent 自主性层级**:
| 级别 | 能力 |
|------|------|
| L1 | 执行明确指令 |
| L2 | 自主测试和验证 |
| L3 | 处理审查反馈 |
| L4 | 端到端交付（复现→修复→PR→合并）|

---

### Stage 4: 验证与审查（Agent + 人类）

**自动验证**（Agent）:
1. 运行 linter（结构检查 + 代码规范）
2. 运行类型检查
3. 运行单元测试
4. 运行集成测试
5. 检查文档同步
6. 生成视频/GIF 演示（UI 变更）

**人类审查**:
1. 审查 PR（代码 + 测试 + 文档）
2. 验证功能（看演示视频）
3. 检查是否符合产品需求
4. 批准或提出修改意见

**Agent 响应反馈**:
- 自动修复审查意见
- 重新运行验证
- 更新 PR

---

### Stage 5: 合并与清理（Agent 执行）

**自动合并**:
1. Agent 监控 CI 状态
2. 自动 rebase 解决冲突
3. 重试 flaky tests
4. 合并到主分支

**知识沉淀**:
1. 更新 `.trellis/spec/`（新认知编码为规范）
2. 更新工作日志 `.trellis/workspace/{name}.journal`
3. 归档任务到 `.trellis/tasks/completed/`
4. 运行质量报告更新

**持续清理**:
- 每周运行垃圾回收任务
- Agent 扫描代码库漂移
- 发起重构 PR
- 更新过时文档

---

## 日常团队节奏

### 每日
- **Agent**: 24/7 运行，自动处理任务队列
- **人类**: 审查昨晚 Agent 提交的 PR

### 每周
- **周一**: 团队同步，分配本周优先级
- **周五**: 代码清理日（垃圾回收）
  - Agent 扫描并发起重构 PR
  - 更新质量评分
  - 审查并合并清理 PR

### 每月
- 审查 `.trellis/spec/` 有效性
- 更新架构文档
- 评估 Agent 自主性提升空间

---

## 关键机制

### 1. 渐进式上下文注入（Trellis）

不是一次性给 Agent 1000 行 AGENTS.md，而是按需注入：

```
任务类型: API 开发
注入上下文:
  - spec/architecture.md（分层规则）
  - spec/api-guidelines.md（接口规范）
  - tasks/active/T123/plan.md（当前计划）
  - workspace/alice.journal（上次工作记录）
```

### 2. DAG 任务编排（Symphony）

```
任务 A: 迁移到 Vite
    ↓（阻塞）
任务 B: 升级 React
    ↓（阻塞）
任务 C: 重构组件库
    ↓
任务 D/E: 并行开发新页面
```

Agent 自动识别依赖，按拓扑顺序执行。

### 3. 黄金原则即代码（Harness）

把团队规范写成 linter，而非文档：

```python
# tools/linters/golden_rules.py
RULES = [
    "禁止直接 dict['key']，必须用 .get() 或 Pydantic",
    "禁止 print()，必须用结构化 logger",
    "禁止超过 300 行的文件",
    "禁止 routes 导入 tests",
    "禁止手写重复工具函数",
]
```

### 4. 仓库即真相

所有知识必须在 git 内：
- ✅ 架构决策 → `docs/architecture/adr-*.md`
- ✅ 设计讨论 → `docs/design/*.md`
- ✅ 团队规范 → `.trellis/spec/*.md`
- ✅ 经验教训 → `.trellis/workspace/*.journal`
- ❌ Slack 讨论、口头约定、个人笔记

### 5. 探索友好

低成本尝试新想法：
- 创建探索性任务 → Agent 自动实现
- 不满意 → 关闭任务，成本接近零
- 满意 → 转化为正式任务

---

## 协作规则

### Agent 之间
- 通过任务系统协作（Linear/GitHub Issues）
- 共享 Spec 作为上下文
- 自动处理依赖关系

### 人类与 Agent
- 人类通过任务描述传递意图
- Agent 通过 PR 返回结果
- 人类通过审查反馈指导

### 人类之间
- PM/设计师直接创建任务（无需懂代码）
- 工程师审查 Agent 输出
- 每周同步调整方向

---

## 工具链

| 类别 | 工具 | 用途 |
|------|------|------|
| 任务管理 | Linear / GitHub Issues | 任务队列、优先级 |
| 代码托管 | GitHub | PR、审查、合并 |
| Agent 平台 | Codex / Claude Code | 代码生成 |
| 规范系统 | Trellis | 上下文注入、工作流 |
| 编排器 | Symphony (可选) | DAG 编排、自动合并 |
| 检查 | 自定义 linter | 强制执行规范 |
| 监控 | PM2 + 日志 | 服务健康 |

---

## 度量指标

| 指标 | 目标 | 说明 |
|------|------|------|
| PR 吞吐量 | 3.5 PR/人/天 | Agent 生成 + 人类审查 |
| 审查时间 | < 30 分钟 | 人类审查每个 PR |
| 测试覆盖率 | > 80% | 自动检查 |
| 文档同步率 | 100% | 变更必须更新文档 |
| Agent 自主性 | L4 | 端到端交付 |
| 探索任务转化率 | > 30% | 尝试 → 保留 |

---

## 快速启动

```bash
# 1. 初始化 Trellis
npm install -g @mindfoldhq/trellis
trellis init -u your-name

# 2. 创建 AGENTS.md（地图模式）
cat > AGENTS.md << 'EOF'
# AGENTS.md - Team Work Map

## Quick Start
1. Read .trellis/spec/architecture.md
2. Check .trellis/tasks/active/ for current work
3. Run `make verify` before starting

## Layer Rules
Models → Config → Services → Routes

## Golden Rules
- Use shared utils (no hand-rolled helpers)
- Validate boundaries (no dict['key'])
- Structured logging (no print)
- Config-driven (no hardcoding)
EOF

# 3. 创建 Makefile
cat > Makefile << 'EOF'
verify:
	python tools/verify.py

test:
	pytest tests/ -v

lint:
	ruff check .
	ruff format .

quality:
	python tools/quality_report.py

clean:
	python tools/garbage_collect.py
EOF

# 4. 运行自检
make verify
```

---

## 参考资源

- [Harness Engineering](https://openai.com/index/harness-engineering/)
- [Symphony Orchestration](https://openai.com/index/open-source-codex-orchestration-symphony/)
- [Trellis Documentation](https://docs.trytrellis.app/zh)
