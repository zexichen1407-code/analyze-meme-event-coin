# Meme 事件叙事分析 Skill

输入一个 Meme coin 的合约地址（CA），让 AI 自动追溯它对应的现实 Meme 或社会事件，重点分析：

- 故事最初怎样发生、为什么成为 Meme；
- 从被嘲讽、质疑、举报或要求下架，到回应、澄清、反转的完整时间线；
- 国内官方或大型媒体是否出现新的原创报道；
- 社交媒体讨论是否跨平台扩散，公众现在如何理解这个故事；
- 目标币是不是当前的叙事/注意力龙头，是否被新版本分流；
- 接下来可能出现哪些转折，以及什么信号能验证或推翻这些路径；
- 其他 Meme、名人或社会事件是否正在抢走同一批注意力。

媒体不需要提到这个币。Skill 追踪的是币背后的现实人物、动物、图片、口号或事件，再单独判断目标币是否仍占据这个叙事。

## 这个 Skill 不做什么

- 不检查合约权限、持仓集中度、LP、滑点、刷量或链上资金；
- 不用币价、市值、成交量、买家数或资金流判断热度；
- 不把新闻热度直接等同于币价上涨；
- 不提供确定性买卖结论。

CA 只用于确认链、名称、ticker 和现实母题。后续热度判断来自社交媒体与文章的内容变化。

## 支持的平台

- ChatGPT/Codex 桌面端中的 Codex；
- Codex CLI 与 IDE 扩展；
- Claude Code CLI、桌面端及 IDE；
- Claude.ai 自定义 Skills。

核心文件 `SKILL.md` 使用 Agent Skills 通用结构。实际分析需要实时访问新闻网页和社交媒体，请为客户端开启必要的互联网与浏览工具权限。

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

### 验证 Codex 安装

新建一个 Codex 任务，然后发送：

```text
使用 $analyze-meme-event-coin 分析这个 CA：<contract-address>
```

也可以直接发送 CA；当任务与 Skill 描述匹配时，Codex 可以自动加载它。

## 安装到 Claude Code

Claude Code 的个人 Skill 目录是 `~/.claude/skills/<skill-name>/SKILL.md`。

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

### 验证 Claude Code 安装

如果本次才首次创建 `~/.claude/skills`，请重启 Claude Code。在 Claude Code 中输入：

```text
/skills
```

确认列表中出现 `analyze-meme-event-coin`，然后调用：

```text
/analyze-meme-event-coin <contract-address>
```

也可以直接说：

```text
请根据社交媒体和大型媒体文章，还原这个 CA 背后事件的完整叙事、转折和未来路径：<contract-address>
```

## 安装到 Claude.ai

Claude.ai 使用打包后的 ZIP。不要上传 GitHub 自动生成的源码 ZIP，请使用 Release 中按 Claude 目录规范整理好的安装包：

[下载 analyze-meme-event-coin.zip](https://github.com/zexichen1407-code/analyze-meme-event-coin/releases/latest/download/analyze-meme-event-coin.zip)

安装步骤：

1. 下载上面的 ZIP，不要解压。
2. 打开 Claude.ai 或 Claude Desktop 的 **Customize > Skills**。
3. 点击 **Add**，上传 `analyze-meme-event-coin.zip`。
4. 上传完成后启用该 Skill。
5. 新建对话并发送一个 CA。

Claude.ai 自定义 Skills 需要启用代码执行。ZIP 顶层是 `analyze-meme-event-coin/`，内部直接包含 `SKILL.md`。

## 如何使用

### 直接输入 CA

```text
分析这个 Meme coin CA：0x...
```

### 强制调用 Codex Skill

```text
使用 $analyze-meme-event-coin 分析：0x...
```

### Claude Code

```text
/analyze-meme-event-coin 0x...
```

通常只需要 CA。只有同一个 EVM 地址在多条链上都存在、无法唯一确认时，Skill 才会继续询问链。

## 分析流程

Skill 会依次完成：

1. **CA 身份映射**：只确认链、名称、ticker 和现实事件，不做合约审计。
2. **叙事起源**：找到最早事件、最早一手材料和它成为 Meme 的原因。
3. **完整时间线**：从首次出现追到当前，而不是只看最近新闻。
4. **关键转折**：主动寻找嘲讽、质疑、走红、举报、下架、回应、澄清和反转。
5. **社交与媒体热度**：观察原创内容、跨平台扩散、二创、评论方向和大型媒体跟进。
6. **当前主流叙事**：说明公众现在相信什么、争论什么，哪些旧说法已被替代。
7. **叙事龙头与分流**：按故事绑定和社交注意力比较同名、跨链及新转折版本。
8. **未来路径**：给出触发条件、领先信号和失效条件。
9. **注意力风险**：检查切割、证伪、下架、敏感化和其他热点抢夺。

预期结果不是一句“还在发酵”，而是一条可验证的故事链，例如：

```text
最初被嘲讽 → 意外走红 → 被要求下架 → 当事方回应 → 新证据出现 → 舆论反转 → 当前争议点
```

每个节点都会尽量附上发生时间、来源、当时公众解释，以及它如何改变后续叙事。

## 热度如何判断

热度只看帖子和文章中的内容信号：

- 是否有新的事实、回应、冲突、处理或反转；
- 是否从一个平台扩展到多个平台和人群；
- 是否有大型媒体、头部账号、机构或品牌加入；
- 是否持续出现原创报道、独立跟进和新二创；
- 评论焦点、关键词和公众解释是否发生变化；
- 新节点是否越来越密集。

点赞、转发、浏览量和热搜排名只能作为辅助证据。负面舆论不等于没有热度：嘲讽、抵制和下架争议也可能推动故事扩散，但必须标明情绪方向。

## 叙事龙头是什么意思

这里的“龙头”不是市值最大，而是：

- 最早且持续绑定核心故事；
- 最贴合当前主流版本和最新转折；
- 在社交讨论中最容易与该事件形成关联；
- 获得当事人、原作者、核心传播者或头部账号提及；
- 社区对其“正统版本”形成相对稳定共识。

可能的结论包括：当前叙事龙头、历史原始版本但注意力已迁移、多版本分裂、目标币绑定较弱、证据不足。

## 结果应该如何理解

Skill 会把以下内容分开：

- **事实**：有一手材料或可靠媒体支持；
- **社交说法**：正在传播但尚未完全确认；
- **分析推断**：基于传播变化做出的判断；
- **未来路径**：等待触发条件验证的情景；
- **缺失证据**：当前无法确认的关键环节。

这是一套叙事研究与注意力风险识别流程，不构成投资建议。

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

Claude.ai 版本请下载最新 Release ZIP 后重新上传。更新后如果行为没有变化，请重启客户端。

## 常见问题

### 为什么媒体报道没有提币？

这是设计目标。Skill 先追踪现实 Meme 母题的完整发展，再判断目标币是否真正占据这个叙事。

### 为什么不检查合约和资金？

当前版本专门研究叙事、社交传播和媒体演化。合约安全、链上资金和交易结构属于另一类分析，不会混入热度结论。

### 为什么不能直接判断一定上涨？

现实事件有热度，不代表注意力一定传导到目标币。目标币可能绑定较弱，也可能被更贴合最新转折的版本分流。

## 仓库结构

```text
analyze-meme-event-coin/
├── SKILL.md
├── README.md
└── agents/
    └── openai.yaml
```

`agents/openai.yaml` 用于 Codex 界面元数据；Claude 安装包只需要通用的 `SKILL.md`。

## 官方参考

- [OpenAI：Build skills](https://learn.chatgpt.com/docs/build-skills)
- [Anthropic：Extend Claude with skills](https://code.claude.com/docs/en/slash-commands)
- [Anthropic：How to create custom skills](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills)