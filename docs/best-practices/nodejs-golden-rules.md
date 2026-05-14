# Node.js + TypeScript 黄金规则

> 完整版本：[team-ai-template-backend/.trellis/spec/golden-rules.md](https://github.com/xiachen332/team-ai-template-backend/blob/main/.trellis/spec/golden-rules.md)

---

## 核心原则摘要

### 类型安全
- 所有函数必须有明确的 TypeScript 类型
- 禁止使用 `any`（除非有充分理由并添加注释）
- 使用接口定义数据结构

### 异步错误处理
- 使用 `async/await` 而非回调
- 所有异步操作必须用 `try-catch` 包裹
- 错误必须传递给统一的错误处理中间件

### 架构分层
```
Models → Services → Controllers → Routes
```

### 安全规范
- 所有用户输入必须验证
- 使用参数化查询或 ORM 防止 SQL 注入
- 敏感数据使用环境变量

### 日志规范
- 使用结构化日志（winston/pino）
- 日志级别：ERROR, WARN, INFO, DEBUG

### 测试要求
- 所有 Service 方法必须有单元测试
- 测试覆盖率 > 80%

---

**详细规则请查看完整文档。**
