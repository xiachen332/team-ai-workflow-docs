# React + TypeScript 黄金规则

> 完整版本：[team-ai-template-frontend/.trellis/spec/golden-rules.md](https://github.com/xiachen332/team-ai-template-frontend/blob/main/.trellis/spec/golden-rules.md)

---

## 核心原则摘要

### 组件设计
- 使用函数组件（不再推荐 class 组件）
- 每个组件只做一件事（单一职责）
- 组件拆分要合理（< 200 行为佳）

### TypeScript 集成
- 所有 Props 必须有明确的类型定义
- 使用正确的事件类型（React.FormEvent, React.ChangeEvent 等）
- API 响应必须有类型定义

### Hooks 规范
- 只在组件顶层调用 Hooks
- 只在 React 函数中调用 Hooks
- 不要在循环、条件或嵌套函数中调用 Hooks

### 状态管理
- 本地状态：useState（表单输入、UI 状态）
- 全局状态：Zustand / Redux Toolkit（用户信息、应用配置）

### 测试规范
- 关键组件测试覆盖率 > 80%
- 使用 @testing-library/react
- 自定义 Hook 必须有测试

### 可访问性（a11y）
- 使用语义化 HTML
- 提供清晰的 ARIA 属性
- 确保键盘导航可用

---

**详细规则请查看完整文档。**
