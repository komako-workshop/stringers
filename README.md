# stringer:把「什么值得做」接进你的 agent

[什么值得做](https://stringers.io) 每天回答一个问题:**一两个人、几个月,现在做什么小产品最可能赚到钱。**

我们每天采集 15 个库:独立产品经支付平台校验的真实收入、App Store 115 个区和 Google Play 的榜单、Meta / Google / TikTok 广告库、GitHub、Product Hunt、Hacker News、X 上几个圈子……在这上面写了约 90 个方向的判断,每个方向一篇长文、一个评级,每天复查。

网页上是写好的判断。接进你自己的 agent(Claude Code、Cursor、Codex、Trae……)之后,它能读这些判断,也能直接去查背后的库,再结合**你自己的情况**回答。

这个仓库是接入用的那一层:说明、Claude Code 插件、skill、命令行。采集和服务端不在这里。

> **English:** Stringers (「什么值得做」, [stringers.io](https://stringers.io)) publishes daily, evidence-backed verdicts on which small-product directions are worth building for indie developers and small teams, drawn from 15 data libraries collected every day (payment-verified indie revenue, App Store / Google Play charts, Meta / Google / TikTok ad libraries, GitHub, Product Hunt, Hacker News, X). This repo connects it to your agent: a remote MCP server (`https://stringers.io/mcp/{token}`, tools `today` / `directions` / `direction` / `data`), a Claude Code plugin, an Agent Skill and a CLI. Content is in Chinese; get a token by logging in at stringers.io.

## 30 秒接上

1. 打开 [stringers.io](https://stringers.io),右上角**登录**(手机号 + 验证码,未注册的自动建号)。
2. 点**配置 agent**,复制那段话,发给你的 agent。
3. 它自己把配置加好。接通以后直接问。

不用往终端里贴命令。那段话里有你的专属地址,长这样:

```
https://stringers.io/mcp/wt_xxxxxxxxxxxxxxxxxxxxxxxx
```

这个地址就是你的 token,别发到公开的地方。泄露了在「配置 agent」里点「换一个新地址」,旧的立刻失效。

## Claude Code 插件

已经有 token(登录后「配置 agent」里那个地址,`/mcp/` 后面 `wt_` 开头的一段),在 Claude Code 里:

```
/plugin marketplace add komako-workshop/stringers
/plugin install stringer@stringers
```

安装时会问 token。MCP 和 skill 一起装好。

## 手动配置

agent 改不了自己的配置,或者你想自己来,按客户端加一个远程 MCP 服务器:名字 `stringer`,类型 Streamable HTTP,地址填上面那个。

**Claude Code**

```bash
claude mcp add --transport http --scope user stringer https://stringers.io/mcp/<token>
```

**Cursor**:`~/.cursor/mcp.json`

```json
{ "mcpServers": { "stringer": { "url": "https://stringers.io/mcp/<token>" } } }
```

**Codex**:`~/.codex/config.toml`

```toml
[mcp_servers.stringer]
url = "https://stringers.io/mcp/<token>"
```

**Trae / Windsurf / Cherry Studio 等**:设置里添加 MCP 服务器,类型选 Streamable HTTP(或「可流式传输的 HTTP」),填地址。只认 JSON 的,用上面 Cursor 那段。

**只支持本地命令(stdio)的客户端**:装下面的命令行,配置成运行 `stringer mcp`。

## 能问什么

- 最近有什么适合用来练手的独立开发项目?
- 今天的头条是什么?跟我有没有关系?
- 我想做一个给 Claude 用的 MCP 工具,这个方向现在谁在做、赚到钱了吗?
- 最近 30 天收入涨得最快的 AI 视频类独立产品是哪几个?各自怎么获客的?
- 按我的情况出一份今天的简报:只看适合 iOS 独立开发、不碰买量的方向。

## 工具

| 工具 | 做什么 |
|---|---|
| `today` | 今天的简报:头条和再看三条(评级、判断、钱、链接) |
| `directions` | 全部方向的一行摘要,可按评级(`grade="SA"`)或关键词(`q`)筛 |
| `direction` | 一个方向的完整判断:结论、为什么这个级、钱在哪、谁在做、什么情况算判断错了、各维度证据 |
| `data` | 直接查一个原始库(只读):`lib` 是库名,`args` 是参数;第一次查某个库先 `args="schema"` |

**评级 = 成功率**:一两个人、几个月,把它做出来并赚到钱的概率。主看窗口期(是不是刚起来)和有多赚钱(有没有校验过的实收、有几家在赚)。

S 正在爆发 · A 值得一试 · B 可能有机会 · C 值得参考(方向别照做,打法值得学) · D 希望较小 · E 不太靠谱

### 能查的库

| 库 | 内容 |
|---|---|
| `indie` | 独立产品经支付平台校验的真实收入(约 1.2 万个产品,逐日 / 逐月) |
| `forsale` | 在售的小生意(Acquire / Flippa):要价、年收入、利润,卖家自填 |
| `appstore` | App Store 115 个区的榜单、评分、评论、新上架(没有下载量和收入) |
| `googleplay` | Google Play 30 个区:榜单、精确安装量、评分 |
| `macappstore` | Mac App Store 20 个区的免费 / 付费榜 |
| `chromestore` | Chrome 商店扩展:用户数、评分、收费迹象 |
| `shopifyapps` | Shopify 应用商店:定价、评分、评论速度 |
| `github` | GitHub ≥50 星的库约 85 万个,每天的星数,trending |
| `producthunt` | Product Hunt 发布、票数、makers(2013 年起) |
| `hackernews` | Hacker News 故事与分数、Show HN(2024 年起) |
| `metaads` | Meta 广告库:谁在买量、在投多少条、落地到哪 |
| `googleads` | Google 广告透明中心:赢家名单上的产品在不在 Google 买量 |
| `tiktokads` | TikTok 广告库(只有投欧洲的) |
| `crossing` | 过线名单:每天在 7 个库里算「今天刚冒头的」 |
| `x` | X 上五个圈子(AI 创始人 / 独立开发者 / 研究员 / 美股投资人 / VC)的原创帖,也能现查 X |

各库的表、字段、口径和边界,让 agent 用 `data` 查 `schema`;数据到哪天看 [stringers.io/status.html](https://stringers.io/status.html)。

## 限额

测试阶段免费。每个 token 每天查库 200 次,其中 X 实时查询 50 次;每次结果最多 50 行。判断层(`today` / `directions` / `direction`)不算在查库次数里。

我们会记下你的 agent 查了什么,以及它转述的「你这次想弄清什么」(工具的可选参数 `ask`,一句话、不带个人信息),用来知道大家关心什么、该补哪些数据;手机号只用来认出你。这些都不会公开。

## 命令行(可选)

一个 Python 文件,只用标准库。

```bash
curl -fsSL https://raw.githubusercontent.com/komako-workshop/stringers/main/cli/stringer -o ~/.local/bin/stringer && chmod +x ~/.local/bin/stringer

stringer today                  # 今天的简报
stringer list --grade SA        # 正在爆发 / 值得一试
stringer show 043               # 一个方向的全部判断
export STRINGER_KEY=wt_…        # 查库的 token(或写到 ~/.config/stringer/key;整条地址也行)
stringer libs                   # 能查的库
stringer schema indie           # 先看口径和边界
stringer indie top --min 1000   # 参数就是各库命令的参数
stringer mcp                    # 以 stdio MCP 跑
```

`today` / `list` / `show` / `status` 不要 token。

## skill(可选)

[`skills/stringer/SKILL.md`](skills/stringer/SKILL.md) 教 agent 怎么用这些判断:四种常见问法怎么答、评级怎么转述、什么时候去查库。接了 MCP 就够用;想让回答更稳,再装它:

- Claude Code:装上面的插件就带了;或者把 `skills/stringer/` 拷到 `~/.claude/skills/stringer/`
- Cursor:内容放进 `.cursor/rules/stringer.mdc`

没接 MCP 也能用:skill 会让 agent 直接读站点的公开 json(`/api/today.json`、`/api/all.json`),只是查不了库。

## 反馈

数字不对、某个方向判断错了、想要哪个库、哪个客户端接不上:开 [issue](https://github.com/komako-workshop/stringers/issues)。

## 许可

本仓库代码 MIT。网站上的文章、判断和数据版权归「什么值得做」所有,引用请注明出处并附链接。
