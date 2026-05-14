# 团队 AI 驱动开发实施指南

> 从 0 到 1 落地，渐进式推进，每阶段都有明确产出

---

## 实施原则

1. **渐进式**：不要一次性做完，每阶段验证有效再推进
2. **从现有项目开始**：用 kiro-gateway 做试点
3. **先基础设施，后流程**：工具先到位，习惯再养成
4. **团队逐步适应**：让每个人看到好处，自然接受

---

## Phase 1: 基础设施搭建（1-2 天）

**目标**：让仓库对 Agent 友好，人类也能受益

### Day 1: 仓库改造

```bash
# 1. 进入项目目录
cd D:\红线工作区\epos\kiro-gateway

# 2. 创建目录结构
mkdir -p docs/{architecture,design,workflows,quality,troubleshooting}
mkdir -p .trellis/{spec,tasks/active,tasks/completed,workspace}
mkdir -p tools/linters

# 3. 初始化 Trellis（如果要用）
npm install -g @mindfoldhq/trellis
trellis init -u redline
```

### 创建核心文件

**AGENTS.md**（已创建，需微调）
- 确保 < 100 行
- 包含：Quick Start、目录结构、分层规则、常用命令

**.trellis/spec/golden-rules.md**
```markdown
# 黄金原则

## 强制规则（linter 检查）
1. 禁止直接 dict['key']，必须用 .get() 或 Pydantic
2. 禁止 print()，必须用 logger
3. 禁止超过 300 行的文件
4. 禁止 routes 导入 tests
5. 禁止手写重复工具函数，用 kiro/utils/

## 架构规则
- 依赖方向：Models → Config → Services → Routes
- 每个业务域分层：Types → Config → Repo → Service → Runtime
```

**Makefile**（已创建，确保可用）
```makefile
.PHONY: setup dev test lint format type-check verify check deploy health diagnose quality-update clean

# 开发
setup:
	pip install -r requirements.txt

dev:
	python start.py

# 测试
test:
	pytest tests/ -v

# 代码检查
lint:
	ruff check .

format:
	ruff format .

type-check:
	mypy kiro/ --ignore-missing-imports

# 验证
verify:
	python tools/verify.py

check: lint type-check test verify

# 部署
deploy:
	@echo "Deploying..."
	pm2 restart ecosystem.config.js

health:
	curl -s http://localhost:8000/health | python -m json.tool

# 诊断
diagnose:
	python tools/diagnose.py

quality-update:
	python tools/quality_report.py

# 清理
clean:
	find . -type d -name __pycache__ -exec rm -rf {} + 2>/dev/null || true
	find . -type f -name "*.pyc" -delete
```

**tools/verify.py**（已创建，测试通过）
- 检查 AGENTS.md、目录结构、依赖方向、测试、健康端点

### 验证

```bash
make verify
# 期望输出：[PASS] All checks passed
```

**产出**：✅ Agent 可以独立工作的基础环境

---

## Phase 2: 单 Agent 跑通（1 周）

**目标**：完成第一个完整任务，验证流程可行

### Step 1: 选一个简单任务

建议选：
- 添加一个 API 端点（如 `/v1/models/{id}`）
- 修复一个已知 bug
- 添加单元测试覆盖

### Step 2: 创建任务文档

```bash
mkdir -p .trellis/tasks/active/T001-add-model-detail
cat > .trellis/tasks/active/T001-add-model-detail/prd.md << 'EOF'
# T001: 添加模型详情接口

## 需求
添加 GET /v1/models/{model_id} 接口，返回模型详细信息

## 验收标准
- [ ] 接口返回模型名称、提供商、能力列表
- [ ] 404 时返回标准错误格式
- [ ] 有单元测试覆盖
- [ ] 更新 API 文档
EOF
```

### Step 3: Agent 执行

给 Agent（我）的指令：
```
任务：完成 .trellis/tasks/active/T001-add-model-detail/prd.md

要求：
1. 先读 AGENTS.md 了解项目结构
2. 按照分层规则实现
3. 运行 make check 确保通过
4. 提交 PR
```

### Step 4: 人类审查

检查点：
- [ ] 代码是否符合分层规则？
- [ ] 测试是否覆盖边界情况？
- [ ] 文档是否同步更新？
- [ ] 是否引入了重复代码？

### Step 5: 合并并归档

```bash
# Agent 自动执行
git add .
git commit -m "feat(routes): add GET /v1/models/{id} endpoint"
git push fork main

# 归档任务
mv .trellis/tasks/active/T001-add-model-detail \
   .trellis/tasks/completed/
```

**产出**：✅ 第一个 Agent 完成的任务，流程验证通过

---

## Phase 3: 团队适配（2-4 周）

**目标**：多人协作，任务跟踪，形成习惯

### 3.1 引入任务跟踪器

选择 Linear（推荐）或 GitHub Issues：

```
Linear 项目结构：
├── Backlog（待办）
├── In Progress（进行中）
├── In Review（审查中）
├── Done（已完成）
└── Archive（归档）
```

### 3.2 团队分工

| 角色 | 工具 | 日常工作 |
|------|------|----------|
| PM | Linear | 创建任务，写需求，验收 |
| 设计师 | Figma + Linear | 设计稿，附加到任务 |
| 工程师 | GitHub + IDE | 审查 PR，维护规范 |
| Agent | CLI | 执行任务，提交 PR |

### 3.3 工作流程

```
每日节奏：
09:00  Agent 拉取 Linear "In Progress" 任务，开始工作
       人类审查昨晚 Agent 提交的 PR
12:00  午餐前快速同步（15分钟）
       - 昨天完成了什么
       - 今天计划做什么
       - 有什么阻塞
18:00  Agent 提交当日 PR，更新任务状态
       人类做最终审查
```

### 3.4 规范沉淀

每周五下午：团队一起审查 `.trellis/spec/`

```bash
# 检查哪些规则被违反了
cat > tools/check_golden_rules.py << 'EOF'
#!/usr/bin/env python3
"""检查黄金原则遵守情况"""
import re
from pathlib import Path

RULES = {
    "no_dict_access": {
        "pattern": r"\[['\"][^'\"]+['\"]\]",
        "message": "发现直接 dict['key'] 访问",
    },
    "no_print": {
        "pattern": r"^\s*print\(",
        "message": "发现 print() 语句",
    },
}

def check():
    root = Path("kiro")
    for py_file in root.rglob("*.py"):
        content = py_file.read_text()
        for name, rule in RULES.items():
            matches = re.finditer(rule["pattern"], content, re.MULTILINE)
            for m in matches:
                print(f"[WARN] {py_file}:{m.start()//100}: {rule['message']}")

if __name__ == "__main__":
    check()
EOF
```

**产出**：✅ 团队协作顺畅，Agent 成为正式成员

---

## Phase 4: 自动化升级（持续）

**目标**：减少人工干预，提升自主性

### 4.1 CI/CD 集成

**.github/workflows/agent-ci.yml**
```yaml
name: Agent CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install ruff mypy pytest
      
      - name: Run checks
        run: |
          make lint
          make type-check
          make test
          make verify
      
      - name: Check golden rules
        run: python tools/linters/golden_rules.py
      
      - name: Update quality report
        run: make quality-update
```

### 4.2 自动合并（可选，谨慎）

```python
# tools/auto_merge.py
"""Agent 自动合并 PR"""

import subprocess

def auto_merge(pr_number):
    """自动合并符合条件的 PR"""
    # 检查条件
    checks = [
        check_ci_passed(pr_number),
        check_review_approved(pr_number),
        check_no_conflicts(pr_number),
        check_tests_pass(pr_number),
    ]
    
    if all(checks):
        subprocess.run(["gh", "pr", "merge", str(pr_number), "--squash"])
        return True
    return False
```

### 4.3 质量监控

**每周自动报告**
```bash
# cron 任务或 GitHub Action
echo "0 9 * * 1 cd /project && make quality-update" | crontab
```

### 4.4 自主性提升

| 阶段 | 自主性 | 人类干预 |
|------|--------|----------|
| L1 | 执行明确指令 | 每步监督 |
| L2 | 自主测试验证 | 审查结果 |
| L3 | 处理审查反馈 | 最终批准 |
| L4 | 端到端交付 | 异常处理 |

**产出**：✅ 团队进入"自动驾驶"模式

---

## 常见问题

### Q: 团队成员抵触怎么办？

**策略**：
1. 先让 Agent 做大家不想做的脏活（写测试、补文档）
2. 展示 Agent 完成的 PR，让大家看到质量
3. 不强制，让自愿者先用起来
4. 每周分享会，展示 Agent 产出

### Q: Agent 做错了怎么办？

**策略**：
1. 不要手动修复，让 Agent 重新做
2. 把问题记录到 `.trellis/spec/`（转化为规则）
3. 更新 linter 防止再犯
4. 视每次错误为改进机会

### Q: 代码质量下降？

**策略**：
1. 严格审查前 20 个 PR（设定基调）
2. 每周五代码清理日
3. 质量报告公开（团队可见）
4. 把品味编码到 linter

### Q: 任务冲突？

**策略**：
1. 使用 Linear 的依赖关系（阻塞/被阻塞）
2. 每个任务独立分支
3. Agent 自动 rebase
4. 复杂任务人工协调

---

## 快速检查清单

### 启动前
- [ ] AGENTS.md 创建且 < 100 行
- [ ] Makefile 可用（make verify 通过）
- [ ] .trellis/spec/golden-rules.md 创建
- [ ] 目录结构符合规范
- [ ] 第一个试点任务选定

### 第一周
- [ ] 第一个 Agent 任务完成并合并
- [ ] 团队看过 Agent 的 PR
- [ ] 至少一次周五代码清理
- [ ] 更新过一次 Spec

### 第一个月
- [ ] 团队 50% 任务由 Agent 处理
- [ ] CI 自动化运行
- [ ] 质量报告每周更新
- [ ] 团队适应新工作流

---

## 最小可行实施（MVP）

如果资源有限，只做这 5 件事：

1. ✅ 创建 AGENTS.md（1 小时）
2. ✅ 创建 Makefile（30 分钟）
3. ✅ 创建 tools/verify.py（2 小时）
4. ✅ 选一个简单任务让 Agent 完成（1 天）
5. ✅ 团队审查并合并（1 小时）

**总共：2 天即可验证可行性**

---

## 参考

- 完整工作流：`D:\红线工作区\team-ai-development-workflow.md`
- Harness Engineering 分析：`D:\红线工作区\epos\kiro-gateway\docs\workflows\development.md`
- Trellis 文档：https://docs.trytrellis.app/zh
