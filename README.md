# stringer:把「什么值得做」接进你的 agent

[什么值得做](https://stringers.io) 每天回答一个问题:**一两个人、几个月,现在做什么小产品最可能赚到钱。**

我们每天采集 15 个库:独立产品经支付平台校验的真实收入、App Store 115 个区和 Google Play 的榜单、Meta / Google / TikTok 广告库、GitHub、Product Hunt、Hacker News、X 上几个圈子……在这上面写了约 90 个方向的判断,每个方向一篇长文、一个评级,每天复查。

网页上是写好的判断。接进你自己的 agent(Claude Code、Cursor、Codex、Trae……)之后,它能读这些判断,也能直接去查背后的库,再结合**你自己的情况**回答。

这个仓库是接入用的那一层:说明、Claude Code 插件、skill、命令行。采集和服务端不在这里。

> **English:** Stringers (「什么值得做」, [stringers.io](https://stringers.io)) publishes daily, evidence-backed verdicts on which small-product directions are worth building for indie developers and small teams, drawn from 15 data libraries collected every day (payment-verified indie revenue, App Store / Google Play charts, Meta / Google / TikTok ad libraries, GitHub, Product Hunt, Hacker News, X). This repo connects it to your agent: a remote MCP server (`https://stringers.io/mcp/{token}`, tools `today` / `hot` / `products` / `directions` / `direction` / `data`), a Claude Code plugin, an Agent Skill and a CLI. Content is in Chinese; get a token by logging in at stringers.io.

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
- 最近有哪些正在爆的单品?哪个适合我跟?
- 我想做一个给 Claude 用的 MCP 工具,这个方向现在谁在做、赚到钱了吗?
- 最近 30 天收入涨得最快的 AI 视频类独立产品是哪几个?各自怎么获客的?
- 按我的情况出一份今天的简报:只看适合 iOS 独立开发、不碰买量的方向。

## 工具

| 工具 | 做什么 |
|---|---|
| `today` | 今天的简报:头条和再看三条(评级、判断、钱、链接) |
| `hot` | 爆款单品:编辑每天从各库挑的、正在爆而且小团队有机会的单个产品,带数字(来源和日期)、为什么在爆、对你的机会、相关方向;`past=true` 连已下榜的也给 |
| `products` | 在涨的单个产品:移动端 App、GitHub 开源项目、Chrome 插件,每个都研究过为什么跑出来、怎么跟,并给了等级;`sec` 选板块(`apps` / `github` / `chrome`),`grade`、`q` 筛,`key` 读整篇文章;`growth="关键词"` 按「怎么起量」搜案例,`key` 加 `part="growth"` 只读它怎么拿到用户(已经有产品、问怎么推广时用) |
| `directions` | 全部方向的一行摘要,可按评级(`grade="SA"`)或关键词(`q`)筛 |
| `direction` | 一个方向的完整判断:结论、为什么这个级、钱在哪、谁在做、什么情况算判断错了、各维度证据 |
| `data` | 直接查一个原始库(只读):`lib` 是库名,`args` 是参数;第一次查某个库先 `args="schema"` |

**评级 = 成功率**:一两个人、几个月,把它做出来并赚到钱的概率。主看窗口期(是不是刚起来)和有多赚钱(有没有校验过的实收、有几家在赚)。

S 正在爆发 · A 值得一试 · B 可能有机会 · C 值得参考(方向别照做,打法值得学) · D 希望较小 · E 不太靠谱

### 能查的库

| 库 | 内容 |
|---|---|
| [`indie`](https://stringers.io/tools/indie-revenue.html) | 独立产品经支付平台校验的真实收入(约 1.2 万个产品,逐日 / 逐月) |
| [`forsale`](https://stringers.io/tools/businesses-for-sale.html) | 在售的小生意(Acquire / Flippa):要价、年收入、利润,卖家自填 |
| [`appstore`](https://stringers.io/tools/app-store-charts.html) | App Store 115 个区的榜单、评分、评论、新上架(没有下载量和收入) |
| [`googleplay`](https://stringers.io/tools/google-play-installs.html) | Google Play 30 个区:榜单、精确安装量、评分 |
| [`macappstore`](https://stringers.io/tools/mac-app-store.html) | Mac App Store 20 个区的免费 / 付费榜 |
| [`chromestore`](https://stringers.io/tools/chrome-extensions.html) | Chrome 商店扩展:用户数、评分、收费迹象 |
| [`shopifyapps`](https://stringers.io/tools/shopify-apps.html) | Shopify 应用商店:定价、评分、评论速度 |
| [`github`](https://stringers.io/tools/github-stars.html) | GitHub ≥50 星的库约 85 万个,每天的星数,trending |
| [`producthunt`](https://stringers.io/tools/product-hunt.html) | Product Hunt 发布、票数、makers(2013 年起) |
| [`hackernews`](https://stringers.io/tools/hacker-news.html) | Hacker News 故事与分数、Show HN(2024 年起) |
| [`metaads`](https://stringers.io/tools/meta-ad-library.html) | Meta 广告库:谁在买量、在投多少条、落地到哪 |
| [`googleads`](https://stringers.io/tools/google-ads-transparency.html) | Google 广告透明中心:赢家名单上的产品在不在 Google 买量 |
| [`tiktokads`](https://stringers.io/tools/tiktok-ad-library.html) | TikTok 广告库(只有投欧洲的) |
| [`crossing`](https://stringers.io/tools/breakouts.html) | 过线名单:每天在 7 个库里算「今天刚冒头的」 |
| [`x`](https://stringers.io/tools/x-circles.html) | X 上五个圈子(AI 创始人 / 独立开发者 / 研究员 / 美股投资人 / VC)的原创帖,也能现查 X |

每个库点进去是一页说明:能问什么、一次真实查询今天返回了什么、这个库不能证明什么([全部 15 个库和 5 种用法](https://stringers.io/tools/))。各库的表、字段、口径和边界,让 agent 用 `data` 查 `schema`;数据到哪天看 [stringers.io/status.html](https://stringers.io/status.html)。

## English

**Stringers is an MCP server for product research**: it gives your AI agent (Claude Code, Cursor, Codex…) read access to 15 databases we collect every day, plus our finished verdicts on about 90 small-product opportunities.

Ask your agent things like:

- Which indie video tools make more than $5,000 a month, and how do they get users?
- Which apps released in the last 60 days are gaining installs fastest?
- Where do my competitors advertise — Meta, Google or TikTok — and are they scaling up?
- I do front-end, solo, two hours a night. What should I build?

| Database | What it holds |
|---|---|
| [`indie`](https://stringers.io/en/tools/indie-revenue.html) | Payment-verified revenue for ~12,000 indie products, daily and monthly |
| [`forsale`](https://stringers.io/en/tools/businesses-for-sale.html) | Small online businesses listed on Acquire / Flippa: price, revenue, profit |
| [`appstore`](https://stringers.io/en/tools/app-store-charts.html) | App Store charts for 115 regions, ratings, reviews, new releases |
| [`googleplay`](https://stringers.io/en/tools/google-play-installs.html) | Google Play charts for 30 stores with exact install counts |
| [`macappstore`](https://stringers.io/en/tools/mac-app-store.html) | Mac App Store free / paid charts for 20 regions |
| [`chromestore`](https://stringers.io/en/tools/chrome-extensions.html) | Chrome extensions: users, ratings, signs of monetisation |
| [`shopifyapps`](https://stringers.io/en/tools/shopify-apps.html) | Shopify apps: pricing, ratings, review velocity |
| [`github`](https://stringers.io/en/tools/github-stars.html) | Daily star history for ~850,000 repositories |
| [`producthunt`](https://stringers.io/en/tools/product-hunt.html) | Product Hunt launches, votes and makers since 2013 |
| [`hackernews`](https://stringers.io/en/tools/hacker-news.html) | Hacker News stories and Show HN since 2024 |
| [`metaads`](https://stringers.io/en/tools/meta-ad-library.html) | Meta Ad Library: who is buying ads, how many, landing where |
| [`googleads`](https://stringers.io/en/tools/google-ads-transparency.html) | Google Ads Transparency Center for products on our watchlist |
| [`tiktokads`](https://stringers.io/en/tools/tiktok-ad-library.html) | TikTok ad library (ads served in Europe only) |
| [`crossing`](https://stringers.io/en/tools/breakouts.html) | Daily breakout list computed across 7 databases |
| [`x`](https://stringers.io/en/tools/x-circles.html) | Posts from five X circles, plus live X search |

Each link shows example questions, a real query with today's output, and what the data cannot prove. Workflows: [product research](https://stringers.io/en/tools/product-research-with-claude-code.html) · [validate an idea](https://stringers.io/en/tools/validate-a-product-idea.html) · [competitor ads](https://stringers.io/en/tools/competitor-ad-research.html) · [rising apps](https://stringers.io/en/tools/find-rising-apps.html) · [first users](https://stringers.io/en/tools/get-first-users.html).

Setup is the same as above: a remote MCP server (Streamable HTTP) at `https://stringers.io/mcp/<token>`, tools `today` / `hot` / `products` / `directions` / `direction` / `data`. Free during beta (200 queries a day). The written verdicts are in Chinese with an English site under [stringers.io/en/](https://stringers.io/en/). Sign-up currently verifies a mainland China mobile number by SMS; international sign-up is not open yet.

## 限额

测试阶段免费。每个 token 每天查库 200 次,其中 X 实时查询 50 次;每次结果最多 50 行。判断层(`today` / `hot` / `products` / `directions` / `direction`)不算在查库次数里。

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
