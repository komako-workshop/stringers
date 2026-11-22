# stringer:把「什么值得做」接进你的 agent

[什么值得做](https://stringers.io) 帮做产品的小团队和独立开发者回答两个问题:**现在做什么有机会;哪些小产品这几天突然涨起来了,为什么涨,你能照着学什么。**

我们每天采集 15 个库:独立产品经支付平台校验的真实收入、App Store 115 个区和 Google Play 的榜单、Meta / Google / TikTok 广告库、GitHub、Product Hunt、Hacker News、X 上几个圈子……在这上面每天做这几件事:

- **赛道和新苗子**:把 App Store 和 Google Play 上做同一件事的新产品归成四百多条赛道,每条知道谁先做出量、谁第几个跟进来、跟的人赚没赚到;另有三百多个已经在赚钱、还没人跟的新产品。
- **App 库**:两个商店约 52 万个 App 的估算月收入、增长、评分、投放、开发者体量和商店介绍全文,可以按条件筛、按意思搜、跑 SQL。
- **市场动态**:从几千个候选里挑出数据上突然起来的小产品(App、开源项目、插件、网站、独立产品收入),逐条调查为什么是这几天涨、这波能不能持续,查完站不住的撤下。
- **打法**:从这些调查里归纳出小团队能照着做的做法,每条有分步骤、真实案例和照做却没成的反例。
- **在涨的产品**:三千多个逐个研究过的 App、GitHub 项目、Chrome 插件、网站,每个写了它靠什么拿到用户,给了跟进等级。

网页上是整理好的榜单和调查。接进你自己的 agent(Claude Code、Cursor、Codex、Trae……)之后,它能读这些调查,也能直接去查背后的库,再结合**你自己的情况**回答。

这个仓库是接入用的那一层:说明、Claude Code 插件、skill、命令行。采集和服务端不在这里。

> **English:** Stringers (「什么值得做」, [stringers.io](https://stringers.io)) helps small teams and indie developers decide what to build: which app niches are being followed by newcomers who actually earn, estimated revenue and ad activity for ~520,000 apps, and which small products suddenly took off each day and why, investigated one by one. It runs on 15 databases collected daily (payment-verified indie revenue, App Store / Google Play charts, Meta / Google / TikTok ad libraries, GitHub, Product Hunt, Hacker News, X). This repo connects it to your agent: a remote MCP server (`https://stringers.io/mcp/{token}`, tools `list_waves` / `get_wave` / `search_apps` / `list_apps` / `get_app` / `query_sql` / `ads` / `feed` / `plays` / `products` / `data`), a Claude Code plugin, an Agent Skill and a CLI. English site: [stringers.io/en/](https://stringers.io/en/); sign in with Google to get a token.

## 30 秒接上

1. 打开 [stringers.io](https://stringers.io),**登录**(手机号 + 验证码,或 Google 账号;未注册的自动建号)。
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

- 现在哪些 App 方向新进去的人还赚得到钱?有哪些已经在赚钱、还没人跟的新产品?
- 做「按预算排一周晚饭」这件事的 App 有多少个?各自赚多少、第几个进场?
- 这个 App(贴商店链接)赚多少、怎么做起来的、在不在买量?
- 今天有哪些小产品突然涨起来了?各自是为什么涨的?
- 最近靠投广告起量的 App 有哪些?它们的素材是怎么写的?
- 我做了一个开源工具,没人知道。有没有能照着做的起量打法?
- 最近 30 天收入涨得最快的 AI 视频类独立产品是哪几个?各自怎么获客的?
- 我想做一个给 Claude 用的 MCP 工具,现在谁在做、赚到钱了吗?
- 我一个人、会前端、每晚两小时,最近涨起来的东西里哪个我能跟?

## 工具

| 工具 | 做什么 |
|---|---|
| `list_waves` | 赛道和早鸟新苗子的列表,找方向先用它。赛道 = 一件具体的事,先有一家做出量,别人再一家家跟进来做。默认按「近 3 个月新进且赚到钱的家数」排,每次 10 条、`offset` 翻页;`kind="solo"` 是已经在赚钱、还没人跟的新产品;`q` 搜关键词,字面没命中会按意思找 |
| `get_wave` | 一条赛道的完整档案:全部成员、各自月入、逐月进场几家;给任意 App 的 key、商店链接或名字,反查它在哪条赛道、第几个进场 |
| `search_apps` | 在约 52 万个 App 里按一句话或名字找,按相关度排:`query` 直接写它是做什么的,或者 App 名字(有别字也行);可叠收入、上架时间、小团队等条件 |
| `list_apps` | 按条件筛一批 App,按收入 / 增长 / 新旧排序,开头给总数,能翻页 |
| `get_app` | 一个 App 的档案:默认精简(估算月入和区间、所在赛道、各平台在不在投广告、调查稿摘要),`sections` 按需加调查稿全文、商店介绍、同开发者的其他 App、近两周走势 |
| `query_sql` | 对 App 库跑一条只读 DuckDB SQL,算分布和统计 |
| `feed` | 市场动态:数据上突然涨起来的小产品,每条都逐个调查过为什么是这几天涨。不带参数给最新一天;`day` 看某一天;`q` 按短词在全部日期里搜;`slug` 读一条的完整调查(定位、对手、时间线、出处、哪些没查实) |
| `plays` | 打法:从这些调查里归纳的做法,每条有分步骤、真实案例(带数字和日期)、照做却没成的反例、适合谁、什么时候不灵。不带参数给清单;`id="p04"` 读全文;`q` 搜 |
| `ads` | 深扒一个 App 的买量素材:它在 Meta(Facebook / Instagram)、Google(搜索 / YouTube / Play)、TikTok(只有投欧洲的)、苹果 App Store 搜索广告(只有 iOS、投欧盟的)上投什么广告:每个平台在投 / 已停 / 查过没投 / 没查过,在投条数、近 7 / 30 天新上、最久投了几天;投得最久和最新的广告(文案、CTA、版本数、视频、广告库原页链接)。`app` = 商店链接、App Store id、Play 包名或名字。只想知道它在不在买量,`apps(key=…)` 里已有摘要。花费不公开,我们不估 |
| `products` | 研究过的在涨产品:三千多个移动端 App、GitHub 开源项目、Chrome 插件、网站,每个都研究过为什么跑出来、用户从哪来,并给了等级。主要用来按获客做法搜实证案例:`growth="关键词"` 按「怎么起量」搜,`key` 加 `part="growth"` 只读它怎么拿到用户(已经有产品、问怎么推广时用);`sec` 选板块(`apps` / `github` / `chrome` / `web`),`grade`、`q` 筛,`key` 读整篇文章。找 App 的方向用 `waves`,筛全量 App 用 `apps` |
| `data` | 直接查一个原始库(只读):`lib` 是库名,`args` 是参数;第一次查某个库先 `args="schema"` |

**怎么选**:找方向用 `list_waves`,看一条赛道的成员用 `get_wave`;找某个或某类 App 用 `search_apps`,要按数字排的清单用 `list_apps`;扒一个 App 用 `get_app`,要逐条看广告再用 `ads`;最近什么在涨用 `feed`;已经有产品、问怎么拿到用户用 `plays` 和 `products(growth=…)`;算分布用 `query_sql`,核对原始数字用 `data`。

**App 的收入**都是模型估算,只当量级看,常见误差两三倍;「赚到钱」指估算月入过 1000 美元。**产品的等级**(`products`):现在跟进它是不是好机会,也就是一两个人、几个月做出来并赚到钱的概率。S 最值得跟,E 别碰。GitHub 项目按「能不能也拿到 star」判断,不谈钱。

> 「爆款单品」(工具 `hot`)2026-11-07 起停止更新,工具已撤下;找方向用 `list_waves`,看最近在涨的用 `feed`。以前的约 90 个「方向」长文和评级(工具 `today` / `directions` / `direction`)2026-11-12 起停止更新,工具也已撤下,存档还在 [stringers.io/all.html](https://stringers.io/all.html)。

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

**Stringers is an MCP server for product research**: it gives your AI agent (Claude Code, Cursor, Codex…) read access to 15 databases we collect every day, plus what we build on top of them: 400+ app niches showing who started each one, who followed and whether the followers earn; estimated revenue and ad activity for ~520,000 apps; daily investigations into which small products suddenly took off and why; and the playbooks we distilled from them.

Ask your agent things like:

- Which app niches are newcomers still making money in? Which new apps earn already and have no look-alike yet?
- How many apps do budget-based meal planning, what does each earn, and who entered when?
- This app (paste a store link): what does it earn, how did it grow, is it buying ads?
- Which small products took off this week, and what drove each one?
- Which indie video tools make more than $5,000 a month, and how do they get users?
- Which apps released in the last 60 days are gaining installs fastest?
- Where do my competitors advertise — Meta, Google or TikTok — and are they scaling up?
- I do front-end, solo, two hours a night. What should I build?

| Tool | What it does |
|---|---|
| `list_waves` | Niches and early birds; start here when looking for a direction. A niche is one specific job that one app proved first and others then followed. Sorted by how many entrants of the last 3 months are earning; 10 per page; `kind="solo"` lists earning apps with no look-alike yet |
| `get_wave` | One niche in full: every member, its revenue estimate and entry month; pass any app key, store link or name to find its niche and entry position |
| `search_apps` | Search ~520,000 apps by a plain-language description or a (possibly misspelled) name, ranked by relevance; combine with revenue, age and team-size filters |
| `list_apps` | Filter apps by conditions, sorted by revenue / growth / age, with a total count and pagination |
| `get_app` | One app's profile: concise by default (revenue estimate and range, niche, ad status per platform, research summary); `sections` adds the full write-up, store description, sibling apps, daily trend |
| `query_sql` | Read-only DuckDB SQL over the app library |
| `ads` | A deep look at one app's ad creatives on Meta, Google, TikTok (Europe only) and Apple Search Ads (EU only): status per platform, live ad count, longest-running and newest ads with copy, CTA and ad-library links |
| `feed` | Daily breakouts: small products that suddenly took off (apps, open-source projects, extensions, websites, indie revenue), each investigated for why it happened now |
| `plays` | Playbooks distilled from those investigations: steps, real cases with numbers and dates, counter-examples, who it fits |
| `products` | 3,000+ researched products across apps, GitHub, Chrome extensions and websites; `growth="regex"` searches them by how they acquired users |
| `data` | Read-only access to the 15 underlying databases below |

Revenue figures for apps are model estimates: read them as orders of magnitude (errors of 2–3x are common). "Earning" means an estimated $1,000+ a month.

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

Setup is the same as above: a remote MCP server (Streamable HTTP) at `https://stringers.io/mcp/<token>`, tools `list_waves` / `get_wave` / `search_apps` / `list_apps` / `get_app` / `query_sql` / `ads` / `feed` / `plays` / `products` / `data` (all read-only and annotated as such, so clients like Codex do not ask for approval on every call). The former `hot` tool stopped updating on 2026-11-07 and has been removed. Free during beta (200 queries a day). Tool output is in Chinese (your agent answers in your language); the site is in English under [stringers.io/en/](https://stringers.io/en/). Get your personal URL by signing in with Google at [stringers.io/en/start.html](https://stringers.io/en/start.html).

## 限额

测试阶段免费。每个 token 每天查库 200 次,其中 X 实时查询 50 次;每次结果最多 50 行。`list_waves` / `get_wave` / `search_apps` / `list_apps` / `get_app` / `query_sql` / `feed` / `plays` / `products` / `ads` 不算在查库次数里。

我们会记下你的 agent 查了什么,以及它转述的「你这次想弄清什么」(工具的可选参数 `ask`,一句话、不带个人信息),用来知道大家关心什么、该补哪些数据;手机号或 Google 账号只用来认出你。这些都不会公开。

## 命令行(可选)

一个 Python 文件,只用标准库。

```bash
curl -fsSL https://raw.githubusercontent.com/komako-workshop/stringers/main/cli/stringer -o ~/.local/bin/stringer && chmod +x ~/.local/bin/stringer

export STRINGER_KEY=wt_…        # token(或写到 ~/.config/stringer/key;整条地址也行)
stringer waves                  # 赛道:近 12 个月首发的,按「近 3 个月新进且赚到钱的家数」排
stringer waves -q '菜谱|meal' --min-r3e 1   # 搜赛道,至少 1 家新进的赚到钱
stringer waves --solo           # 早鸟新苗子:已经在赚钱、还没人跟(--include-uncloneable 连政务 / 银行类也给)
stringer waves --key ios_6759298323   # 一条赛道的完整档案;给任意 App 的 key / 商店链接则反查它第几个进场
stringer apps --key https://apps.apple.com/us/app/…/id6759298323   # 一个 App 的全景档案
stringer apps --pattern 'kaleidoscope|万花筒' --rev-min 1000 --small-team   # 按意思找一类 App
stringer apps --sql "select category, count(*) n from apps where revenue>=10000 and age_days<=180 group by 1 order by n desc"
stringer apps --category 语言学习 --rev-min 10000 --age-max 180   # 按条件筛;--stats 语言学习 看品类概况
stringer feed                   # 市场动态,最新一天
stringer feed -q 广告            # 在全部日期里搜
stringer feed <slug>            # 一条动态的完整调查
stringer plays                  # 打法清单
stringer plays p04              # 一条打法全文
stringer ads com.freereels.app  # 一个 App 在 Meta / Google / TikTok / 苹果投什么广告(也收商店链接、App Store id、名字)
stringer products --sec apps --grade AB   # 在涨的产品
stringer libs                   # 能查的库
stringer schema indie           # 先看口径和边界
stringer indie top --min 1000   # 参数就是各库命令的参数
stringer mcp                    # 以 stdio MCP 跑
```

除了 `status` 都要 token;没有 token 时 `stringer feed` 退回读首页的公开 Markdown 版。`stringer waves --help`、`stringer apps --help` 看全部参数。`list` / `show <编号>` 还能读已停更的方向存档;`stringer hot` 已停更,会提示改用 `waves` 或 `feed`。

## skill(可选)

[`skills/stringer/SKILL.md`](skills/stringer/SKILL.md) 教 agent 怎么用这些工具:找方向、验想法、扒竞品、找获客做法这几种问法各自怎么查、估算和等级怎么转述、什么时候去查库。接了 MCP 就够用;想让回答更稳,再装它:

- Claude Code:装上面的插件就带了;或者把 `skills/stringer/` 拷到 `~/.claude/skills/stringer/`
- Cursor:内容放进 `.cursor/rules/stringer.mdc`

没接 MCP 也能用一部分:skill 会让 agent 读首页的公开 Markdown 版(`https://stringers.io/index.md`,近几天的市场动态)和 `/api/apps.json` 这类公开 json,只是读不了每条的完整调查和打法,也查不了库。

## 反馈

数字不对、某条调查的原因找错了、想要哪个库、哪个客户端接不上:开 [issue](https://github.com/komako-workshop/stringers/issues)。

## 许可

本仓库代码 MIT。网站上的文章、判断和数据版权归「什么值得做」所有,引用请注明出处并附链接。
