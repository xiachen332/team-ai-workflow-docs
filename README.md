# Team AI Workflow Documentation

> 团队 AI 驱动开发工作流文档中心

---

## 📚 文档导航

### 核心文档

| 文档 | 说明 | 适合人群 |
|------|------|----------|
| [工作流文档](docs/workflow.md) | 5 阶段工作流、团队角色、仓库结构 | 所有人 |
| [技能开发指南](docs/skills/creating-team-skills.md) | 如何创建和使用团队技能 | 工程师 |

### 实施指南

| 文档 | 说明 | 技术栈 |
|------|------|--------|
| [Python 项目实施指南](docs/implementation/python-projects.md) | Python 项目落地步骤 | Python + FastAPI/Flask |
| [Node.js/React 实施指南](docs/implementation/nodejs-react-projects.md) | Node.js + React 项目落地步骤 | Node.js + React |

### 最佳实践

| 文档 | 说明 | 技术栈 |
|------|------|--------|
| [Node.js 黄金规则](docs/best-practices/nodejs-golden-rules.md) | Node.js + TypeScript 编码规范 | Node.js |
| [React 黄金规则](docs/best-practices/react-golden-rules.md) | React + TypeScript 编码规范 | React |

---

## 🚀 快速开始

### 第一步：选择实施指南

**Python 项目团队**：
→ [Python 项目实施指南](docs/implementation/python-projects.md)

**Node.js/React 项目团队**：
→ [Node.js/React 实施指南](docs/implementation/nodejs-react-projects.md)

### 第二步：从模板创建项目

```bash
# 后端项目
gh repo create my-backend --template xiachen332/team-ai-template-backend

# 前端项目
gh repo create my-frontend --template xiachen332/team-ai-template-frontend
```

### 第三步：遵循黄金规则

每个模板都包含 `.trellis/spec/golden-rules.md`，遵循这些规则可以确保代码质量。

---

## 🏗️ 文档结构

```
docs/
├── workflow.md                      # 5 阶段工作流
├── implementation/                  # 实施指南
│   ├── python-projects.md          # Python 项目指南
│   └── nodejs-react-projects.md    # Node.js/React 项目指南
├── best-practices/                  # 最佳实践
│   ├── nodejs-golden-rules.md      # Node.js 编码规范
│   └── react-golden-rules.md       # React 编码规范
└── skills/                          # 技能开发
    └── creating-team-skills.md      # 技能创建指南
```

---

## 📦 相关仓库

| 仓库 | 说明 |
|------|------|
| [team-ai-template-backend](https://github.com/xiachen332/team-ai-template-backend) | Node.js 后端项目模板 |
| [team-ai-template-frontend](https://github.com/xiachen332/team-ai-template-frontend) | React 前端项目模板 |
| [team-ai-tools](https://github.com/xiachen332/team-ai-tools) | 团队技能和工具包 |

---

## 🤝 贡献

### 更新文档

1. Fork 本仓库
2. 更新或添加文档
3. 提交 Pull Request

### 添加新技能

参考 [技能开发指南](docs/skills/creating-team-skills.md)

---

## 📋 核心理念

```
人类 = 掌舵者（需求、决策、审查、品味）
Agent = 执行者（编码、测试、文档、部署）
仓库 = 唯一真相源（所有知识必须版本化）
```

---

## 📅 变更历史

| 日期 | 变更内容 |
|------|----------|
| 2026-05-14 | 初始版本，包含工作流、实施指南、黄金规则、技能指南 |

---

**问题或建议？** 提交 Issue 或 Pull Request。
