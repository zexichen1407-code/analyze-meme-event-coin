# Meme 事件币分析 Skill

输入一个 Meme coin 的合约地址（CA），让 Codex 自动追溯它对应的现实 Meme 或社会事件，并分析：

- 现实事件是否仍在发酵；
- 国内官方或大型媒体是否出现新的报道；
- 目标币是否仍是同叙事的存量、增量龙头；
- 是否有同名币、跨链版本或新币正在分流；
- 合约、持仓、流动性和成交是否存在风险；
- 当前币圈重大事件是否会打断注意力与资金传导。

媒体不需要提到这个币。Skill 追踪的是币背后的现实人物、动物、图片、口号或事件，再判断这些新闻是否可能为目标币带来新增注意力。

## 适用环境

- ChatGPT/Codex 桌面端中的 Codex；
- Codex CLI；
- Codex IDE 扩展。

实际分析需要访问实时网页、正规媒体、区块浏览器和市场数据。请确保 Codex 具备相应的互联网访问能力。

## 推荐安装：使用 Skill Installer

在 Codex 中发送：

```text
$skill-installer
请从 https://github.com/zexichen1407-code/analyze-meme-event-coin 安装这个 Skill。
```

安装完成后，新开一个任务测试。如果 Skill 没有立即出现，请重启 Codex。

## 手动安装

Codex 会从用户级目录 `$HOME/.agents/skills` 发现个人 Skill。仓库目录下必须能直接看到 `SKILL.md`。

### Windows PowerShell

```powershell
$skillRoot = Join-Path $HOME ".agents\skills"
New-Item -ItemType Directory -Force -Path $skillRoot | Out-Null
git clone "https://github.com/zexichen1407-code/analyze-meme-event-coin.git" (Join-Path $skillRoot "analyze-meme-event-coin")
```

安装后的关键路径应为：

```text
C:\Users\你的用户名\.agents\skills\analyze-meme-event-coin\SKILL.md
```

### macOS / Linux

```bash
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/zexichen1407-code/analyze-meme-event-coin.git \
  "$HOME/.agents/skills/analyze-meme-event-coin"
```

安装后的关键路径应为：

```text
~/.agents/skills/analyze-meme-event-coin/SKILL.md
```

## 验证是否安装成功

1. 重启 Codex，或新建一个任务。
2. 在 Codex CLI/IDE 中输入 `/skills`，检查是否出现 `analyze-meme-event-coin`。
3. 也可以直接输入 `$`，查看 Skill 选择列表。
4. 发送下面的测试提示：

```text
使用 $analyze-meme-event-coin 分析这个 CA：<contract-address>
```

如果 Skill 能开始识别链、代币和现实 Meme 母题，就说明安装成功。

## 如何使用

最简单的调用方式：

```text
使用 $analyze-meme-event-coin 分析这个 CA：0x...
```

Solana 等非 EVM 链同样可以直接提供 CA：

```text
使用 $analyze-meme-event-coin 分析这个 CA：<Solana mint address>
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

如果通过 Git 克隆手动安装，可以运行：

### Windows PowerShell

```powershell
git -C "$HOME\.agents\skills\analyze-meme-event-coin" pull
```

### macOS / Linux

```bash
git -C "$HOME/.agents/skills/analyze-meme-event-coin" pull
```

更新后如果行为没有变化，请重启 Codex。

## 常见问题

### `/skills` 中找不到 Skill

检查下面的文件是否存在：

```text
$HOME/.agents/skills/analyze-meme-event-coin/SKILL.md
```

常见原因包括：

- 多嵌套了一层目录；
- 仓库没有完整克隆；
- `SKILL.md` 被改名；
- 安装后尚未重启 Codex；
- 存在另一个同名 Skill，选择时需要根据路径区分。

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

## 参考

- [OpenAI 官方文档：Build skills](https://learn.chatgpt.com/docs/build-skills)
