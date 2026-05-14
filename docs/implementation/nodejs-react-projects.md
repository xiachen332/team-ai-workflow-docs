# Node.js + React 项目实施指南

> 基于 2026-05-14 的实践经验总结

---

## 快速开始

### 1. 从模板创建项目

**后端项目（Node.js + Express + TypeScript）**:
```bash
gh repo create my-backend --template xiachen332/team-ai-template-backend
cd my-backend
npm install
npm run dev
```

**前端项目（React + Vite + TypeScript）**:
```bash
gh repo create my-frontend --template xiachen332/team-ai-template-frontend
cd my-frontend
npm install
npm run dev
```

---

## 模板已包含

### .trellis/ 结构

```
.trellis/
├── README.md              # 使用指南
├── spec/
│   ├── golden-rules.md    # 项目黄金规则
│   └── team-resources.md  # 团队资源索引
├── tasks/
│   ├── active/            # 进行中的任务
│   └── completed/         # 已完成的任务
└── workspace/             # 工作日志
```

### AGENTS.md

项目地图文件，包含：
- Quick Start
- 目录结构
- 架构规则
- 常用命令

### Makefile

统一命令入口：
```bash
make install    # 安装依赖
make dev        # 开发模式
make test       # 运行测试
make lint       # 代码检查
make verify     # 运行所有检查
```

---

## 黄金规则

每个模板都有针对其技术栈的黄金规则：

### Node.js 后端
- 类型安全（TypeScript）
- 异步错误处理（async/await + try-catch）
- 分层架构（Models → Services → Controllers → Routes）
- 安全规范（输入验证、SQL 注入防护）
- 日志规范（结构化日志）
- 测试覆盖（> 80%）

详细规则：[team-ai-template-backend/.trellis/spec/golden-rules.md](https://github.com/xiachen332/team-ai-template-backend/blob/main/.trellis/spec/golden-rules.md)

### React 前端
- 函数组件优先
- TypeScript 类型定义
- Hooks 使用规范
- 状态管理选择
- 测试要求
- 可访问性（a11y）

详细规则：[team-ai-template-frontend/.trellis/spec/golden-rules.md](https://github.com/xiachen332/team-ai-template-frontend/blob/main/.trellis/spec/golden-rules.md)

---

## 技能集成

### 可用技能

查看团队技能仓库：[team-ai-tools](https://github.com/xiachen332/team-ai-tools)

### 使用技能

1. 克隆技能仓库：
```bash
git clone https://github.com/xiachen332/team-ai-tools.git
```

2. 复制需要的技能到项目：
```bash
cp -r team-ai-tools/skills/team/minimax-tts your-project/skills/
```

3. 配置环境变量（如需要）：
```bash
export MINIMAX_API_KEY="your-api-key"
```

### 创建新技能

参考技能模板：[team-ai-tools/skills/team/_template](https://github.com/xiachen332/team-ai-tools/tree/main/skills/team/_template)

---

## 常见问题

### Q: npm install 失败怎么办？

**症状**：安装过程中被 SIGKILL 终止

**原因**：内存或资源不足

**解决**：
- 增加系统可用内存
- 使用 `npm install --prefer-offline` 减少内存占用
- 分批安装依赖

### Q: HUSKY 钩子失败怎么办？

**症状**：commit 时报错 `Cannot find module 'pnpm'`

**原因**：模板配置了 pnpm，但系统未安装

**解决**：
```bash
# 方法 1：跳过钩子
git commit --no-verify -m "message"

# 方法 2：禁用 HUSKY
HUSKY=0 git commit -m "message"

# 方法 3：安装 pnpm
npm install -g pnpm
```

### Q: 如何添加团队资源？

编辑 `.trellis/spec/team-resources.md`，添加团队仓库地址、API 文档等。

---

## 相关资源

- [团队工作流文档](../workflow.md)
- [Python 项目实施指南](./python-projects.md)
- [技能开发指南](../skills/creating-team-skills.md)
- [Node.js 黄金规则](../best-practices/nodejs-golden-rules.md)
- [React 黄金规则](../best-practices/react-golden-rules.md)

---

## 变更历史

| 日期 | 变更内容 |
|------|----------|
| 2026-05-14 | 初始版本，基于实践总结 |
