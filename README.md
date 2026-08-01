# make-knowledge-cards

一个可开源的 WorkBuddy Skill：把用户粘贴的文章或本地 `Markdown/TXT` 文件，提炼为 **5~8 张知识卡片**，便于快速回顾与自测。

## 1. 项目解决什么问题

长文、长笔记读完容易忘，也不方便检验自己到底懂没懂。本 Skill 把一篇文章「压缩」成几张**只讲一个知识点**的卡片：每张卡用「核心知识 + 简明解释 + 例子 / 自测」把关键结论讲清楚，让你能快速复习、随时自测，而不必重读全文。

它只做**信息提炼与重组**——不联网、不抓取、不转换格式、不渲染界面，因此输出稳定、可控、可复现。

## 2. 主要功能

- **输入灵活**：支持直接粘贴文章文本，或给出本地 `.md` / `.txt` 文件路径。
- **结构化输出**：生成一组 Markdown 知识卡片，每张包含四个字段：
  - **标题** —— 一句话点明知识点（建议 ≤ 15 字）
  - **核心知识** —— 精炼结论（是什么 / 关键结论是什么）
  - **简明解释** —— 1~3 句背景、原理或「为什么重要」
  - **例子或自测问题** —— 具体例子，或检验理解的问题
- **智能控制数量**：目标 5~8 张；真正重要的点不足 5 个时**如实减少并说明原因**，绝不强行凑数；超过 8 个只保留最重要的 8 个。
- **去重与聚焦**：含义相同或高度重叠的内容只保留一个知识点；一张卡片只讲一个知识点。
- **严格忠实原文**：所有内容必须来自你提供的材料，不编造原文没有的事实、数据、人名、年份或结论。
- **文件输入可落盘**：若输入为本地文件，卡片会同时写入源文件同目录的 `<原名>_cards.md`（不覆盖已有文件）。

## 3. 安装方法

本 Skill 仅依赖自身的指令文件，**无需额外脚本、无需安装第三方依赖**。

### 在 WorkBuddy 中使用
将 `make-knowledge-cards` 目录放到 Skills 目录下即可（二选一）：

- 用户级：`~/.workbuddy/skills/make-knowledge-cards/`
- 项目级：`<你的项目>/.workbuddy/skills/make-knowledge-cards/`

例如（Windows）：

```powershell
# 用户级
Copy-Item -Recurse skills/make-knowledge-cards "$env:USERPROFILE\.workbuddy\skills\"

# 或项目级
Copy-Item -Recurse skills/make-knowledge-cards ".workbuddy\skills\"
```

### 在 OpenAI Agents SDK 环境中使用
`agents/openai.yaml` 是与 `SKILL.md` 规则保持一致的 agent 定义，可被遵循 Agents 规范的运行环境加载。将整个 `make-knowledge-cards` 目录作为该 agent 的来源，运行时会读取 `agents/openai.yaml` 中的 instructions。

## 4. 使用方法

1. 把 `make-knowledge-cards` 安装到 Skills 目录（见上文）。
2. 在对话中触发，两种常见方式：
   - **粘贴文本**：直接把文章贴进对话，说「帮我生成知识卡片」。
   - **本地文件**：给出文件路径，例如「把 `笔记.md` 转成知识卡片」。
3. Skill 会按规则提炼并返回 Markdown 卡片（开头一行总览，如「已从原文提炼 N 张知识卡片」）。
4. 若输入为本地文件，卡片会同时写入 `<原名>_cards.md`；若你只想要对话结果，明确说「不用写文件」即可。

## 5. 输入输出示例

**输入（节选）**

> 费曼技巧是一种帮助理解复杂概念的学习方法。它的核心是用自己的话把知识讲清楚。
> 第二步，假装教给一个完全不懂的外行，用最通俗的语言解释这个概念。
> 第三步，如果在讲解中卡壳，就回到资料重新学习薄弱环节。

**输出**

已从原文提炼 3 张知识卡片：

## 卡片 1：费曼技巧的定位

- **核心知识**：费曼技巧是用自己的话讲清楚知识、而非死记硬背的理解型学习方法。
- **简明解释**：它把「能否讲明白」作为是否真懂的检验标准。
- **例子 / 自测**：自测——「为什么能背下公式不等于真懂？」

## 卡片 2：第二步——对外行讲解

- **核心知识**：假装教给完全不懂的外行，用最通俗的语言解释。
- **简明解释**：通俗化会暴露你含混的地方，逼你真正理解。
- **例子 / 自测**：自测——「如果你只能用术语解释，算讲清楚了吗？」

## 卡片 3：第三步——卡壳就回头补

- **核心知识**：讲解中卡壳或说不清时，回到资料补齐薄弱环节。
- **简明解释**：卡壳处正是理解漏洞，针对性回看比通读更高效。
- **例子 / 自测**：例子——讲到某处卡住，就专门补这一块。

> 完整示例见 [`skills/make-knowledge-cards/examples/`](skills/make-knowledge-cards/examples/)。

## 不支持的功能

明确不实现，也不会尝试绕过：

- **网页抓取 / 在线抓取文章链接**：本 Skill 不访问网络。
- **PDF 解析**：仅接受 `.md` 与 `.txt`。
- **Anki 导出**：不生成 `.apkg` 或其他间隔重复软件格式。
- **图形界面（GUI）**：仅以文本（Markdown）输出。

若你有上述需求，可先用「复制网页 / PDF 文本进来再生成卡片」，或导出 Markdown 后自行导入 Anki。

## 目录结构

```
skills/
├── make-knowledge-cards/
│   ├── SKILL.md
│   ├── agents/
│   │   └── openai.yaml
│   └── examples/
│       ├── input.md
│       └── output.md
└── publish-to-github/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    └── scripts/
        └── publish_to_github.ps1
```

## GitHub 一键发布

本仓库还提供了一个辅助 Skill：`publish-to-github`。它用于把本地 Git 项目按标准流程发布到 GitHub，包括：

- 检查 `gh` 是否已安装并登录
- 创建远程仓库并推送当前分支
- 创建 tag
- 创建 GitHub Release

### 快速开始

```powershell
powershell -ExecutionPolicy Bypass -File .\skills\publish-to-github\scripts\publish_to_github.ps1 -RepoName article-to-knowledge-cards -Visibility public -TagName v0.1.0 -ReleaseTitle "v0.1.0"
```

### 常见参数

- `-RepoName`：GitHub 仓库名
- `-Visibility`：`public` / `private`
- `-TagName`：版本号，如 `v0.1.0`
- `-ReleaseTitle`：GitHub Release 标题
- `-ReleaseNotes`：发布说明
- `-Branch`：目标分支，默认 `main`

### 常见问题

- 如果提示 `gh` 未安装：先安装 GitHub CLI
- 如果提示未登录：浏览器授权后再重试
- 如果当前目录不是 Git 仓库：先执行 `git init` 与 `git commit`

## 开源说明

本 Skill 仅依赖 Skill 自身的指令（无额外脚本），`SKILL.md` 与 `agents/openai.yaml` 规则保持一致，可直接用于 WorkBuddy 或遵循 OpenAI Agents 规范的运行环境。欢迎在遵守许可证的前提下自由使用与修改。
