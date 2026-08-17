# Meme 事件币分析 Skill

输入一个 Meme coin 的合约地址（CA），让 AI 自动追溯它对应的现实 Meme 或社会事件，并分析：

- 现实事件是否仍在发酵；
- 国内官方或大型媒体是否出现新的报道；
- 目标币是否仍是同叙事的存量、增量龙头；
- 是否有同名币、跨链版本或新币正在分流；
- 合约、持仓、流动性和成交是否存在风险；
- 当前币圈重大事件是否会打断注意力与资金传导。

媒体不需要提到这个币。Skill 追踪的是币背后的现实人物、动物、图片、口号或事件，再判断这些新闻是否可能为目标币带来新增注意力。

## 支持的平台

- ChatGPT/Codex 桌面端中的 Codex；
- Codex CLI 与 IDE 扩展；
- Claude Code CLI、桌面端及 IDE；
- Claude.ai 自定义 Skills。

核心文件 `SKILL.md` 使用 Agent Skills 通用结构，仅包含标准的 `name`、`description` 和 Markdown 指令。实际分析需要实时网页、正规媒体、区块浏览器和市场数据，请为所使用的客户端开启必要的互联网与工具权限。

## 安装到 Codex

### 推荐：使用 Skill Installer

在 Codex 中发送：

```text
$skill-installer
请从 https://github.com/zexichen1407-code/analyze-meme-event-coin 安装这个 Skill。
```

安装完成后新开一个任务测试。如果 Skill 没有立即出现，请重启 Codex。

### Windows PowerShell 手动安装

```powershell
$skillRoot = Join-Path $HOME ".agents\skills"
New-Item -ItemType Directory -Force -Path $skillRoot | Out-Null
git clone "https://github.com/zexichen1407-code/analyze-meme-event-coin.git" (Join-Path $skillRoot "analyze-meme-event-coin")
```

关键路径：

```text
C:\Users\你的用户名\.agents\skills\analyze-meme-event-coin\SKILL.md
```

### macOS / Linux 手动安装

```bash
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/zexichen1407-code/analyze-meme-event-coin.git \
  "$HOME/.agents/skills/analyze-meme-event-coin"
```

关键路径：

```text
~/.agents/skills/analyze-meme-event-coin/SKILL.md
```

### 验证 Codex 安装

1. 新建一个 Codex 任务。
2. 在 CLI/IDE 中输入 `/skills`，确认出现 `analyze-meme-event-coin`。
3. 或输入 `$`，从 Skill 列表中选择它。
4. 发送：

```text
使用 $analyze-meme-event-coin 分析这个 CA：<contract-address>
```

## 安装到 Claude Code

Claude Code 的个人 Skill 目录是 `~/.claude/skills/<skill-name>/SKILL.md`。安装后，该 Skill 可用于你本机的所有 Claude Code 项目。

### Windows PowerShell

```powershell
$skillRoot = Join-Path $HOME ".claude\skills"
New-Item -ItemType Directory -Force -Path $skillRoot | Out-Null
git clone "https://github.com/zexichen1407-code/analyze-meme-event-coin.git" (Join-Path $skillRoot "analyze-meme-event-coin")
```

关键路径：

```text
C:\Users\你的用户名\.claude\skills\analyze-meme-event-coin\SKILL.md
```

### macOS / Linux

```bash
mkdir -p "$HOME/.claude/skills"
git clone https://github.com/zexichen1407-code/analyze-meme-event-coin.git \
  "$HOME/.claude/skills/analyze-meme-event-coin"
```

关键路径：

```text
~/.claude/skills/analyze-meme-event-coin/SKILL.md
```

### 验证 Claude Code 安装

如果安装时 `~/.claude/skills` 已存在，Claude Code 通常会实时发现新 Skill；如果本次才首次创建顶层 `skills` 目录，请重启 Claude Code。

在 Claude Code 中输入：

```text
/skills
```

确认列表中出现 `analyze-meme-event-coin`，然后调用：

```text
/analyze-meme-event-coin <contract-address>
```

也可以不使用斜杠命令，直接说：

```text
请分析这个 Meme coin 的 CA：<contract-address>，判断现实事件热度、龙头地位、分流和风险。
```

Claude 会根据 Skill 的 `description` 判断是否自动加载。

## 安装到 Claude.ai

Claude.ai 使用打包后的 ZIP。不要直接上传 GitHub 自动生成的源码 ZIP，请使用仓库 Release 中已经按 Claude 要求整理好的安装包：

[下载 analyze-meme-event-coin.zip](https://github.com/zexichen1407-code/analyze-meme-event-coin/releases/latest/download/analyze-meme-event-coin.zip)

安装步骤：

1. 下载上面的 ZIP，不要解压。
2. 打开 Claude.ai 或 Claude Desktop 的 **Customize > Skills**。
3. 点击 **Add**，上传 `analyze-meme-event-coin.zip`。
4. 上传完成后启用该 Skill。
5. 新建对话并发送：

```text
请分析这个 Meme coin 的 CA：<contract-address>，判断现实事件是否仍在发酵，以及它是不是当前龙头。
```

Claude.ai 的自定义 Skills 功能需要启用代码执行。Release ZIP 的顶层是 `analyze-meme-event-coin/` 文件夹，内部直接包含 `SKILL.md`，符合 Claude 的打包要求。

## 如何使用

### Codex

```text
使用 $analyze-meme-event-coin 分析这个 CA：0x...
```

### Claude Code

```text
/analyze-meme-event-coin 0x...
```

### Solana 等非 EVM 链

```text
分析这个 CA：<Solana mint address>
```

通常只需要 CA。Skill 会自行识别链；只有同一个 EVM 地址在多条链上都存在、无法唯一确认时，才会继续询问所在链。

## 分析流程

Skill 会依次完成：

1. **身份核验**：识别链、名称、ticker、创建时间、部署者和主要交易池。
2. **现实母题追溯**：确认币对应的人物、动物、图片、口号或社会事件。
3. **权威新闻搜索**：搜索现实事件本身，不要求新闻出现币名或 CA。
4. **发酵度判断**：区分加速发酵、持续发酵、停滞、结束、降温或证据不足。
5. **龙头与分流**：分别判断存量龙头、增量龙头，并寻找同名、跨链和新转折版本。
6. **自身风险**：检查合约权限、大户卖出、LP、流动性、刷量和社区风险。
7. **外部冲击**：检查市场清算、交易所、公链、稳定币、监管和注意力迁移事件。
8. **传导结论**：把新闻事实、市场推断、失效条件和缺失证据分开输出。

## 结果应该如何理解

Skill 不会把“新闻还在发酵”直接等同于“币价一定上涨”。最终结论会区分：

- 事件继续发酵，目标币仍是龙头；
- 事件继续发酵，但新增资金被竞争币分流；
- 叙事有效，但币本身存在合约或流动性风险；
- 现实热度仍在，但币圈整体风险正在压制价格；
- 当前证据不足，无法确认目标币会受益。

这是一套研究与风险识别流程，不构成投资建议。

## 更新 Skill

### Codex 手动安装版本

```powershell
git -C "$HOME\.agents\skills\analyze-meme-event-coin" pull
```

### Claude Code 手动安装版本

Windows PowerShell：

```powershell
git -C "$HOME\.claude\skills\analyze-meme-event-coin" pull
```

macOS / Linux：

```bash
git -C "$HOME/.claude/skills/analyze-meme-event-coin" pull
```

Claude.ai 版本请从 Releases 下载新的 ZIP 后重新上传。更新后如果行为没有变化，请重启对应客户端。

## 常见问题

### Codex 找不到 Skill

检查：

```text
$HOME/.agents/skills/analyze-meme-event-coin/SKILL.md
```

### Claude Code 找不到 Skill

检查：

```text
$HOME/.claude/skills/analyze-meme-event-coin/SKILL.md
```

然后运行 `/skills`。如果安装前不存在 `~/.claude/skills` 顶层目录，请重启 Claude Code。

### Claude.ai 上传失败

- 使用 Releases 中的 `analyze-meme-event-coin.zip`，不要上传散落文件；
- 不要先解压 ZIP；
- 确认 ZIP 中第一层是 `analyze-meme-event-coin/`，其内部直接包含 `SKILL.md`；
- 确认 Claude 账户已启用自定义 Skills 和代码执行。

### 为什么媒体报道没有提币？

这是设计目标，不是错误。Skill 追踪现实 Meme 母题的后续发展，再单独判断注意力是否可能传导到目标币。

### 为什么不能直接判断一定上涨？

现实事件热度只是第一层。目标币还可能被竞争币分流，或者受到大户卖出、流动性不足、合约权限、市场清算和注意力迁移等影响。

## 仓库结构

```text
analyze-meme-event-coin/
├── SKILL.md
├── README.md
└── agents/
    └── openai.yaml
```

其中 `agents/openai.yaml` 用于 Codex 的界面元数据；Claude 安装包只需要通用的 `SKILL.md`。

## 官方参考

- [OpenAI：Build skills](https://learn.chatgpt.com/docs/build-skills)
- [Anthropic：Extend Claude with skills](https://code.claude.com/docs/en/slash-commands)
- [Anthropic：How to create custom skills](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills)
