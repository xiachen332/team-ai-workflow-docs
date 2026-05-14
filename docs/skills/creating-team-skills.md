# 创建团队技能指南

> 如何创建、维护和使用团队自定义技能

---

## 什么是技能（Skill）？

技能是 AI Agent 的**可复用能力模块**，包含：
- **SKILL.md** - 技能说明文档
- **工具脚本** - PowerShell、Python 等
- **模板文件** - 可复用的模板
- **示例** - 使用案例

---

## 快速开始

### 使用技能模板

技能模板位置：[team-ai-tools/skills/team/_template](https://github.com/xiachen332/team-ai-tools/tree/main/skills/team/_template)

```bash
# 克隆技能仓库
git clone https://github.com/xiachen332/team-ai-tools.git

# 复制模板
cp -r team-ai-tools/skills/team/_template team-ai-tools/skills/team/your-skill-name

# 编辑 SKILL.md
cd team-ai-tools/skills/team/your-skill-name
# 填写技能信息
```

---

## SKILL.md 必需内容

每个技能的 `SKILL.md` 必须包含：

| 内容 | 说明 |
|------|------|
| 名称 | 清晰、描述性的名称 |
| 描述 | 详细说明技能的功能 |
| 触发条件 | 什么时候使用这个技能 |
| 使用方法 | 如何调用和使用 |
| 示例 | 具体的使用案例 |

---

## 安全最佳实践

### ⚠️ 不要硬编码敏感信息

**错误示例**：
```powershell
# ❌ 不要这样做！
$API_KEY = "sk-abc123..."
```

**正确示例**：
```powershell
# ✅ 使用环境变量
$API_KEY = $env:MY_API_KEY

if (-not $API_KEY) {
    Write-Error "MY_API_KEY environment variable is not set"
    exit 1
}
```

### 环境变量配置

在 SKILL.md 中说明如何配置：

```markdown
## 环境配置

设置环境变量：

\`\`\`powershell
$env:MY_API_KEY = "your-api-key-here"
\`\`\`
```

---

## 实战案例：minimax-tts

### 技能结构

```
minimax-tts/
├── SKILL.md     # 文档
└── tts.ps1      # PowerShell 脚本
```

### 关键设计点

1. **使用环境变量**：
   ```powershell
   $API_KEY = $env:MINIMAX_API_KEY
   ```

2. **验证环境变量**：
   ```powershell
   if (-not $API_KEY) {
       Write-Error "MINIMAX_API_KEY not set"
       exit 1
   }
   ```

3. **清晰的文档**：
   - 说明支持的功能
   - 提供使用示例
   - 列出可用选项

---

## 文件命名规范

### 目录命名
- 使用小写字母和连字符：`daily-report`, `code-review`
- 避免特殊字符
- 名称应清晰表达功能

### 文件命名
- `SKILL.md` - 必需，技能文档
- `*.ps1` - PowerShell 脚本
- `*.py` - Python 脚本
- `*.sh` - Bash 脚本

---

## 技能分类

### team/ - 团队专属技能
- 团队自己编写
- 包含团队特定逻辑
- 可能包含敏感配置

### public/ - 公开技能
- 从社区收集
- 已适配团队需求
- 标注来源和许可证

---

## 提交技能

### 检查清单

- [ ] SKILL.md 包含所有必需内容
- [ ] 没有硬编码的敏感信息
- [ ] 提供了至少一个使用示例
- [ ] 添加了环境变量配置说明
- [ ] 列出了注意事项和限制

### 提交流程

```bash
cd team-ai-tools
git add skills/team/your-skill-name/
git commit -m "feat: add your-skill-name skill"
git push
```

---

## 相关资源

- [技能模板](https://github.com/xiachen332/team-ai-tools/tree/main/skills/team/_template)
- [minimax-tts 示例](https://github.com/xiachen332/team-ai-tools/tree/main/skills/team/minimax-tts)
- [Node.js/React 实施指南](../implementation/nodejs-react-projects.md)

---

## 变更历史

| 日期 | 变更内容 |
|------|----------|
| 2026-05-14 | 初始版本，包含安全最佳实践 |
