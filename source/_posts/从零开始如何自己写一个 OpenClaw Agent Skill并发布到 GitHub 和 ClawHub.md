---
title: '从零开始：如何自己写一个 OpenClaw Agent Skill，并发布到 GitHub 和 ClawHub'
date: 2026-06-28 21:51:21
tags: [openclaw, skills, agent]
mathjax: true
comments: true
---

> 让 AI Agent 学会新本事，不需要写一行代码——只需要写一份 Markdown。

## 什么是 Skill？

OpenClaw Skill 是一种教 AI Agent 如何使用工具、何时调用工具的机制。每个 Skill 本质上是一个包含 `SKILL.md` 文件的目录，里面用 YAML frontmatter 定义元数据，用 Markdown 正文给 Agent 下发操作指令。

当你用自然语言跟 Agent 对话时，它会根据你的请求自动匹配并加载最相关的 Skill，然后按照你写好的步骤去执行。

**Skill 的核心优势：**

- 零代码：纯 Markdown 编写，谁都能上手
- 即写即用：写完保存，Agent 自动感知
- 可分享：发布到 ClawHub，全世界的人都能安装
- 可条件加载：根据环境、依赖、平台动态决定是否启用

---

## 第一步：创建你的第一个 Skill

### 1. 创建目录结构

OpenClaw 的 Skill 存放在工作区的 `skills/` 目录下：

```bash
mkdir -p ~/.openclaw/workspace/skills/my-first-skill
```

你可以按子目录组织 Skill，比如 `skills/research/` 或 `skills/personal/`，Skill 的名称来自 `SKILL.md` 的 frontmatter，与文件夹路径无关。

### 2. 编写 SKILL.md

在目录下创建 `SKILL.md`：

```markdown
---
name: hello-agent
description: 一个演示 Skill，向用户打招呼并展示当前时间
---

# Hello Agent

当用户想要打招呼或询问时间时，使用 `exec` 工具运行以下命令：

```bash
echo "你好！现在是 $(date '+%Y-%m-%d %H:%M:%S')"
```

## 注意事项

- 用户可能用中文或英文打招呼
- 如果用户明确询问某个时区的时间，加上 `TZ=指定时区 date` 命令
```

**Frontmatter 必填字段：**

| 字段 | 说明 | 规则 |
|---|---|---|
| `name` | Skill 的唯一标识 | 小写字母、数字、连字符 |
| `description` | 一句话描述，用于匹配和展示 | 单行，建议 160 字符以内 |

**可选字段：**

| 字段 | 说明 |
|---|---|
| `user-invocable` | 是否暴露为斜杠命令（默认 `true`） |
| `disable-model-invocation` | 是否从 Agent 系统提示中隐藏（默认 `false`） |
| `homepage` | 项目主页 URL，显示在 macOS Skills UI 中 |
| `metadata.openclaw` | 条件门控、安装器等（单行 JSON 格式） |

### 3. 验证 Skill 已加载

```bash
openclaw skills list
```

如果没看到，开一个新会话或重启 Gateway：

```bash
openclaw gateway restart
```

### 4. 测试

直接在聊天中让 Agent 触发你的 Skill，或者显式调用：

```bash
openclaw agent --message "跟我打个招呼"
```

也可以在对话中使用 `/hello-agent` 斜杠命令。

---

## 第二步：编写更强大的 Skill

### 使用 `{baseDir}` 引用同级文件

如果 Skill 需要附带脚本、模板等辅助文件，用 `{baseDir}` 引用：

```
目录结构：
my-skill/
├── SKILL.md
├── scripts/
│   └── check.sh
└── templates/
    └── report.md
```

```markdown
---
name: daily-report
description: 生成每日工作报告
---

# 每日报告

运行报告脚本：

```bash
bash {baseDir}/scripts/check.sh
```

报告模板在：

```
{baseDir}/templates/report.md
```

```

### 条件加载（Gating）

让 Skill 只在特定条件下加载，比如只有当某个依赖存在时才启用：

```markdown
---
name: gemini-search
description: 使用 Gemini CLI 进行搜索
metadata: { "openclaw": { "requires": { "bins": ["gemini"], "env": ["GEMINI_API_KEY"] } } }
---
```

**可用的门控规则：**

| 规则 | 说明 |
|---|---|
| `requires.bins` | 所有指定的二进制文件必须在 PATH 上 |
| `requires.anyBins` | 至少一个二进制文件在 PATH 上 |
| `requires.env` | 环境变量必须存在 |
| `requires.config` | OpenClaw 配置中的指定路径必须为真 |
| `os` | 平台过滤：`["darwin"]`、`["linux"]`、`["win32"]` |
| `always` | 设为 `true` 跳过所有门控 |

### 安装器规格（Installer Specs）

告诉 macOS Skills UI 如何自动安装依赖：

```markdown
---
name: my-tool
description: 使用 my-tool 完成某件事
metadata:
  {
    "openclaw":
      {
        "emoji": "🛠️",
        "requires": { "bins": ["mytool"] },
        "install": [
          {
            "id": "brew",
            "kind": "brew",
            "formula": "my-tool",
            "bins": ["mytool"],
            "label": "Install my-tool (brew)"
          }
        ]
      }
  }
---
```

### 配置覆盖

在 `openclaw.json` 中覆盖 Skill 的行为：

```json5
{
  skills: {
    entries: {
      "my-skill": {
        enabled: true,
        env: { API_KEY: "your-key-here" },
        config: { endpoint: "https://example.com" }
      }
    }
  }
}
```

---

## 第三步：通过 Skill Workshop 提案

如果你希望 Agent 起草 Skill、经过审核后再生效，可以使用 **Skill Workshop**。这是一种受治理的 Skill 创建路径。

### 用 CLI 提案创建

```bash
openclaw skills workshop propose-create \
  --name "daily-report" \
  --description "每日工作报告生成" \
  --proposal ./PROPOSAL.md
```

PROPOSAL.md 内容格式：

```markdown
---
name: "daily-report"
description: "每日工作报告生成"
status: proposal
version: "v1"
date: "2026-06-27T00:00:00.000Z"
---

# 每日报告

（你的 Skill 指令内容...）
```

### 提案操作

```bash
# 查看待审核提案
openclaw skills workshop list

# 审查提案内容
openclaw skills workshop inspect <proposal-id>

# 修改提案
openclaw skills workshop revise <proposal-id> --proposal ./PROPOSAL.md

# 审核后生效
openclaw skills workshop apply <proposal-id>

# 拒绝
openclaw skills workshop reject <proposal-id> --reason "不符合规范"

# 隔离审查
openclaw skills workshop quarantine <proposal-id> --reason "需要安全审查"
```

### 用 Agent 工具提案

你也可以直接在聊天中让 Agent 帮你创建提案：

> "帮我创建一个叫 weather-check 的 Skill，描述是'查询天气并提醒带伞'"

Agent 会自动调用 `skill_workshop` 工具并返回提案 ID。

---

## 第四步：发布到 ClawHub

ClawHub（[clawhub.ai](https://clawhub.ai)）是 OpenClaw 的公共 Skill 注册中心。

### 1. 安装 ClawHub CLI

```bash
npm i -g clawhub
clawhub login
```

### 2. 安装发布 Skill（可选）

```bash
openclaw skills install clawhub-publish
```

### 3. 发布 Skill

```bash
# 发布整个 Skill 文件夹
clawhub skill publish ./skills/my-first-skill

# 指定版本号
clawhub skill publish ./skills/my-first-skill --version 1.0.0
```

发布后，你的 Skill 页面就是：

```
https://clawhub.ai/<你的owner>/<slug>
```

### 4. ClawHub 发布流程

1. **收集元数据和文件**：CLI 自动读取 `SKILL.md` 中的 frontmatter
2. **发送发布请求**：包含 owner、slug、版本、更新日志和所有文件
3. **服务器验证**：检查 owner 权限、包名、版本、文件大小和来源
4. **存储发布**：ClawHub 保存新版本
5. **安全检查**：自动运行 VirusTotal、ClawScan 和静态分析
6. **审核通过后上架**：新版本对所有用户可见

### 5. 其他人安装你的 Skill

```bash
# 从 ClawHub 安装
openclaw skills install <your-username>/my-first-skill

# 安装指定版本
openclaw skills install <your-username>/my-first-skill --version 1.0.0

# 全局安装（所有 Agent 可用）
openclaw skills install <your-username>/my-first-skill --global
```

---

## 第五步：发布到 GitHub

### 1. 准备仓库

把你的 Skill 目录放到一个 Git 仓库中：

```bash
cd ~/.openclaw/workspace/skills/my-first-skill
git init
git add SKILL.md
git commit -m "feat: 添加 my-first-skill"
git branch -M main
git remote add origin https://github.com/你的用户名/my-first-skill.git
git push -u origin main
```

### 2. 仓库结构建议

```
my-first-skill/
├── SKILL.md              # 必需：Skill 定义文件
├── README.md             # 可选：对人类的说明文档
├── scripts/              # 可选：辅助脚本
│   └── helper.sh
├── templates/            # 可选：模板文件
│   └── report.md
└── assets/               # 可选：静态资源
    └── logo.png
```

### 3. 通过 Git 安装

其他用户可以直接从 GitHub 安装：

```bash
openclaw skills install git:用户名/仓库名
openclaw skills install git:用户名/仓库名@main
openclaw skills install git:用户名/仓库名@v1.0.0
```

### 4. 自定义 Skill 名称

如果仓库名和 Skill 名不一致，用 `--as` 指定：

```bash
openclaw skills install git:用户名/repo-name --as my-skill-name
```

---

## 最佳实践

### 编写 SKILL.md 的建议

1. **简洁明了**：告诉 Agent 做什么，而不是教它"作为一个 AI"该怎么思考
2. **安全优先**：如果 Skill 使用 `exec`，确保不会注入不可信的输入
3. **描述简短**：`description` 控制在 160 字符以内，减少 Token 消耗
4. **先测试再分享**：用 `openclaw agent --message "..."` 本地验证

### Token 消耗须知

每个 Skill 注入系统提示的 Token 消耗是可预测的：

```
总开销 = 195 + Σ (97 + len(名字) + len(描述) + len(文件路径))
```

约每 4 个字符 ≈ 1 个 Token。保持描述简短就能减少开销。

### 组织多个 Skill

```
skills/
├── research/
│   ├── SKILL.md       # 自动识别为 "research"
│   └── papers/
│       └── SKILL.md   # 自动识别为 "papers"
├── personal/
│   └── SKILL.md       # 自动识别为 "personal"
└── SKILL.md           # 根目录的 SKILL.md 也是有效 Skill
```

### 多 Agent 场景下的 Skill 共享

| 作用域 | 路径 | 对谁可见 |
|---|---|---|
| 单个 Agent | `<workspace>/skills` | 仅该 Agent |
| 项目 Agent | `<workspace>/.agents/skills` | 仅该工作区的 Agent |
| 个人级 | `~/.agents/skills` | 本机所有 Agent |
| 共享级 | `~/.openclaw/skills` | 本机所有 Agent |

---

## 常见问题

### Q：写完 SKILL.md 后 Agent 没反应？

A：开一个新会话（`/new`）或重启 Gateway（`openclaw gateway restart`）。OpenClaw 默认监听 Skill 文件变化，但已有会话的快照不会自动刷新。

### Q：如何调试 Skill 匹配问题？

A：使用以下命令：

```bash
openclaw skills list              # 查看所有 Skill
openclaw skills list --eligible    # 查看符合条件的 Skill
openclaw skills check              # 检查 Agent 实际可见的 Skill
openclaw skills info my-skill      # 查看某个 Skill 的详情
```

### Q：第三方 Skill 安全吗？

A：把第三方 Skill 视为**不受信任的代码**。启用前请先阅读其内容。对不可信的输入，优先使用沙箱运行。

### Q：如何更新已发布的 Skill？

A：更新本地文件后，重新发布到 ClawHub：

```bash
clawhub skill publish ./skills/my-skill --version 1.0.1
```

其他人用 `openclaw skills update <slug>` 即可更新。

---

## 结语

写 OpenClaw Skill 本质上就是写一份给 AI Agent 看的操作手册。你不需要会编程，只需要把你的工作流用清晰的指令描述出来，Agent 就能帮你自动化完成。

从一个小工具开始，逐步扩展你的 Agent 能力。当你的 Skill 变得足够好用时，发布到 ClawHub，让全世界的人受益。

*本文基于 OpenClaw 文档编写。文档地址：https://docs.openclaw.ai*
