# Stringers — Indie Mobile App Builder's AI Skill & MCP

**移动端 UI 设计 Skill + 52 万 App 商业情报 MCP**

[English](#english) · [中文](#中文) · [stringers.io/en/styles](https://stringers.io/en/styles/)

| | Mobile UI design · 移动端 UI 设计 | 520k-app market intelligence · 52 万 App 商业情报 |
|---|---|---|
| What · 是什么 | 204 real iOS UI styles, one `DESIGN.md` each<br>204 套真实 iOS 界面风格，每套一份 `DESIGN.md` | Niches, revenue, reviews, ad creatives, growth playbooks<br>赛道、收入、评论、买量素材、增长打法 |
| How · 怎么用 | Skill, **zero login** · Skill，**免登录** | Remote MCP, **30-second token** · 远程 MCP，**30 秒拿 token** |
| Install · 安装 | `npx skills add komako-workshop/stringers` | [Configure agent](https://stringers.io/en/start.html) · [配置 agent](https://stringers.io) |

---

## English

Stringers ([stringers.io/en](https://stringers.io/en/)) gives your coding agent two things an indie mobile app builder needs: **taste** (204 real iOS UI styles it can build from) and **market data** (what ~520,000 apps earn, and how they got there).

### Quickstart 1: Mobile UI design (zero login)

```bash
npx skills add komako-workshop/stringers
```

Then, in Claude Code, Cursor, Codex or any agent that supports skills:

1. **Say** "redesign this screen with stringers". Same sentence for a brand-new app, an ugly screen, or one that looks AI-generated.
2. **Pick a look.** The AI picks 3 contrasting styles (light and dark, one of them bold) and sends a live `?pick=` link with 4 real iOS screens per style. None of them right? Browse all 204 styles at [stringers.io/en/styles](https://stringers.io/en/styles/) and send any style name back.
3. **Get the code.** It locks that style's `DESIGN.md`, views the 4-screen reference strip, then writes clean SwiftUI / Expo / React Native UI without deleting a single feature. On routine edits afterwards it stays silent and keeps the existing style.

A `?pick=` link looks like this: <https://stringers.io/en/styles/?pick=oat-margin,amber-roundel,citron-kite>

Already on the Claude Code plugin? Nothing else to install: from 0.12.0 the plugin ships the same skill.

### Quickstart 2: 520k-app market intelligence MCP (30-second token)

Stringers is an app intelligence library for indie developers and small teams. It answers two questions: **which directions have room right now, and how did the products that took off get their users and revenue.**

Five complementary data layers:

- **Hot niches (`list_waves` / `get_wave`)**: 400+ niches built from recent App Store and Google Play launches that do the same job. Each shows who got traction first, who followed in what order, and how many entrants from the last 3 months earn ≥ $1K a month.
- **Early-bird apps (`list_early_apps`)**: 300+ recently launched apps that already earn and have no look-alike in the library yet.
- **The full library and single-app research (`search_apps` / `list_apps` / `get_app` / `research_app` / `query_sql`)**: ~520,000 App Store and Google Play apps with monthly revenue, downloads, growth, ratings, languages, in-app purchase ranges, same-developer portfolios, 12.41M real user reviews with 1–2 star pain points, multi-country chart history, release notes, and live plus historical ad creatives from four platforms (Meta / Google / TikTok / Apple). Web research and the write-up are done by your own model.
- **Market feed (`feed`)**: small products whose numbers moved suddenly each day, with an investigation of why.
- **Playbooks (`plays`)**: reusable growth, cold-start and monetization plays distilled from real cases, searchable by acquisition tactic.

#### Connect in 30 seconds

1. Open [stringers.io/en/start.html](https://stringers.io/en/start.html) and **sign in** (phone number + code, or Google; new accounts are created automatically).
2. Copy the setup prompt and paste it to your agent.
3. The agent adds the configuration itself. Once connected, just ask.

Your personal address looks like this:

```
https://stringers.io/mcp/wt_xxxxxxxxxxxxxxxxxxxxxxxx
```

That address is your token, so keep it out of public places. If it leaks, generate a new address on the same page and the old one stops working immediately. There is no shared or example address; a URL containing a placeholder returns 403.

#### Claude Code plugin

Once you have a token, in Claude Code:

```
/plugin marketplace add komako-workshop/stringers
/plugin install stringer@stringers
```

It asks for the token during install and sets up the MCP server and the skill (UI design + market intelligence) together.

#### Manual setup

Add one remote MCP server in your client: name `stringer`, type Streamable HTTP, URL = your personal address.

**Claude Code**

```bash
claude mcp add --transport http --scope user stringer https://stringers.io/mcp/<token>
```

**Cursor**: `~/.cursor/mcp.json`

```json
{ "mcpServers": { "stringer": { "url": "https://stringers.io/mcp/<token>" } } }
```

**Codex**: `~/.codex/config.toml`

```toml
[mcp_servers.stringer]
url = "https://stringers.io/mcp/<token>"
```

**ChatGPT deep research / connectors that only support `search` + `fetch`**: use `https://stringers.io/mcp/oai/<token>`.

**Clients that only run local commands (stdio)**: install the CLI below and configure the client to run `stringer mcp`.

#### What to ask

- Which app niches still pay for newcomers? Which early-bird apps have no follower yet?
- How many apps plan a week of dinners on a budget? What does each earn, who entered when, who is buying ads?
- This app (paste a store link): what does it earn, how did it grow, and which ad creatives does it run on Meta / Google / TikTok / Apple?
- Which products broke out in today's feed? Turn it into a daily brief for my category.
- I shipped a utility app nobody knows about. Which growth plays can I copy, and what are the real cases?

#### Tools (12)

| Tool | What it does |
|---|---|
| `publish_board` | Packs the niches, early-bird apps and apps you selected into an interactive web board (`https://stringers.io/appdb/#b/<id>`) with icons, revenue trends, entry-by-month charts and AI notes |
| `research_app` | Everything the library knows about one app (revenue, review text, charts, release notes, four-platform ad creatives, existing write-ups) plus a checklist of what to look up on the web yourself |
| `compare_apps` | 2–3 competitors side by side: revenue, downloads, main markets, ad activity and real 1–2 star reviews |
| `list_waves` | Hot niches, ranked by how many recent entrants earn; supports `q` search, `category`, `store`, `detail=true`, `offset` |
| `get_wave` | Full profile of one niche: positioning, analysis, month-by-month entries, every earning member; pass any app to find its niche and entry rank |
| `list_early_apps` | Early-bird apps: recently launched, already earning, no look-alike yet; supports `q`, `category`, `store`, `min_rev` and sorting |
| `search_apps` | Fuzzy search across ~520k apps by a sentence or a name, with related niches attached and optional revenue / launch date / small-team / category filters |
| `list_apps` | Filter a batch of apps and sort by revenue, growth or age; returns a total count, supports regex `pattern` and paging |
| `get_app` | Full profile of one app; add `sections` such as `reviews`, `charts`, `release_notes`, `ads`, `research`, `description`, `developer`, `daily`, `wave`, `all`; `check_ads=true` checks Apple + Meta ads live |
| `query_sql` | Read-only DuckDB SQL over the app library for distributions, percentiles and cross-category stats |
| `feed` | Daily breakouts with the reason behind each: latest day by default, `day` for a date, `q` to search, `slug` for the full investigation |
| `plays` | Growth, cold-start and monetization playbooks (`id`, `q`), plus `case_q` to search thousands of real cases by acquisition tactic and `case_key` for one app's details |

#### Limits

Free during the beta: no card required, Google sign-in and international phone numbers are supported. Read-only tools are annotated `readOnlyHint: true`, so clients such as Codex do not ask for confirmation on every call.

#### CLI (optional)

One Python file, standard library only:

```bash
curl -fsSL https://raw.githubusercontent.com/komako-workshop/stringers/main/cli/stringer -o ~/.local/bin/stringer && chmod +x ~/.local/bin/stringer
export STRINGER_KEY=wt_…
stringer waves
stringer waves --solo
stringer search "plan a week of dinners from supermarket prices"
stringer apps --key https://apps.apple.com/us/app/…/id6759298323
stringer feed
stringer plays
```

### What's in this repo

This repo is the access layer: docs, the Claude Code plugin, the skill and the CLI. Data collection and the server are not here.

[`skills/stringers/`](skills/stringers/SKILL.md) is one unified skill that loads only the half you need: [`references/design.md`](skills/stringers/references/design.md) for UI work, [`references/market.md`](skills/stringers/references/market.md) for market research. The design half works without the MCP connected.

### License

Code in this repo is MIT. Articles, judgments and data on the website belong to Stringers (「什么值得做」); please credit and link when quoting.

---

## 中文

### 快速开始 1：移动端 UI 设计（免登录）

```bash
npx skills add komako-workshop/stringers
```

装好以后，在 Claude Code、Cursor、Codex 等支持 skill 的 agent 里：

1. **说一句**「用 stringers 帮我设计这个界面」。新做一个 App、把丑界面换皮、去 AI 味，都是这一句。
2. **挑风格**。AI 挑 3 套反差明显的风格（有浅有深，其中一套大胆的），给你一条 `?pick=` 链接，每套 4 张真实 iOS 截图并排看；都不对味，就去 [stringers.io/styles](https://stringers.io/styles/) 逛全部 204 套，把风格名发回对话。
3. **落代码**。AI 锁定这套风格的 `DESIGN.md`，看一眼 4 屏参考图，再写 SwiftUI / Expo / React Native 界面，原有功能一个不删。之后的日常小改沿用现有风格，不会再跳出来让你选。

`?pick=` 链接长这样：<https://stringers.io/styles/?pick=oat-margin,amber-roundel,citron-kite>

已经装了 Claude Code 插件的不用另装：插件 0.12.0 起自带同一个 skill。

### 快速开始 2：52 万 App 商业情报 MCP（30 秒拿 token）

[什么值得做](https://stringers.io) 是面向独立开发者和小团队的 **App 商业情报库**，帮你回答两个核心问题：**现在做什么方向有机会；跑出来的产品是怎么拿到用户和赚到钱的。**

整套库由五个互补的数据层组成：

- **热门赛道（`list_waves` / `get_wave`）**：把近一两年 App Store 和 Google Play 上做同一件事的新产品归成四百多条赛道，每条知道谁先做出量、谁第几个跟进来、近 3 个月新进的人里有多少家赚到钱（月入 ≥ $1K）——这是目前大家都在做、确定性相对较高的方向。
- **早鸟新苗子（`list_early_apps`）**：近期上架已经开始赚钱、全库还没见到同款的 0 跟进独苗，适合挑出来逐个拆解产品形态、流量来源和可复制性。
- **52 万 App 全量库与单品调研材料（`search_apps` / `list_apps` / `get_app` / `research_app` / `query_sql`）**：App Store 与 Google Play 约 52 万个 App 的月收入、下载、增长、评分、支持语言与内购区间、同开发者矩阵、1,241 万条真实用户评论与 1–2 星差评痛点、多国榜单历史、版本更新日志、四平台（Meta / Google / TikTok / Apple）实时与历史买量素材，深度拆解单个 App 时一次拿齐全套材料，联网调查和成文由你自己的模型完成。
- **市场动态（`feed`）**：每天数据上突然异动的小产品及原因调查，适合追踪最新热点、查某个产品为什么这几天火了，或整理成每日情报日报。
- **打法库（`plays`）**：从大量真实起量案例中归纳出的可复用增长、冷启动和变现做法（含步骤、正反案例），并支持按获客做法直接检索实证案例。

网页上是整理好的榜单和调查。接进你自己的 agent（Claude Code、Cursor、Codex、Trae……）之后，它能直接读这五个数据层，再结合**你自己的情况**给出判断。

#### 30 秒接上

1. 打开 [stringers.io](https://stringers.io)，**登录**（手机号 + 验证码，或 Google 账号；未注册的自动建号）。
2. 点**配置 agent**，复制那段话，发给你的 agent。
3. 它自己把配置加好。接通以后直接问。

你的专属地址长这样：

```
https://stringers.io/mcp/wt_xxxxxxxxxxxxxxxxxxxxxxxx
```

这个地址就是你的 token，别发到公开的地方。泄露了在「配置 agent」里点「换一个新地址」，旧的立刻失效。

#### Claude Code 插件

已经有 token 后，在 Claude Code 里：

```
/plugin marketplace add komako-workshop/stringers
/plugin install stringer@stringers
```

安装时会问 token，MCP 和 skill（UI 设计 + 商业情报）一起装好。

#### 手动配置

按客户端加一个远程 MCP 服务器：名字 `stringer`，类型 Streamable HTTP，地址填你的专属地址。

**Claude Code**

```bash
claude mcp add --transport http --scope user stringer https://stringers.io/mcp/<token>
```

**Cursor**：`~/.cursor/mcp.json`

```json
{ "mcpServers": { "stringer": { "url": "https://stringers.io/mcp/<token>" } } }
```

**Codex**：`~/.codex/config.toml`

```toml
[mcp_servers.stringer]
url = "https://stringers.io/mcp/<token>"
```

**ChatGPT 深度研究 / 只支持 `search` + `fetch` 的连接器**：使用 `https://stringers.io/mcp/oai/<token>`。

**只支持本地命令（stdio）的客户端**：装下面的命令行，配置成运行 `stringer mcp`。

#### 能问什么

- 最近有哪些 App 赛道新进去的人也能赚到钱？哪些早鸟新苗子刚起步还没人跟？
- 做「按预算排一周晚饭」这件事的 App 有多少个？各自月入多少、第几个进场、谁在买量？
- 这个 App（贴商店链接）月入多少、怎么做起来的、在 Meta / Google / TikTok / Apple 投了什么广告素材？
- 今天市场动态里有哪些产品突然涨起来了？帮我整理一份我关心领域的日报。
- 我做了一个工具 App 没人知道，有哪些可照抄的起量打法和实证案例？

#### 工具（12 个）

| 工具 | 做什么 |
|---|---|
| `publish_board` | 把筛选出的赛道、早鸟新苗子和具体 App 打包成一张专属可交互网页清单（`https://stringers.io/appdb/#b/<id>`），带图标、月入走势、赛道图标墙、月度进场柱图和 AI 定制点评，一键点进详情页与广告素材 |
| `research_app` | **一个 App 的全套调研材料**：输入 App Store / Google Play 链接、key 或名字，一次返回库里关于它的全部真值（收入、差评原文、多国榜单、更新日志、四平台广告素材、已有调查稿），并附一张「还需要你自己联网查什么」的清单。只给数据，联网调查和成文由你自己的模型做 |
| `compare_apps` | **2–3 款竞品并排对比**：并发拉取收入、下载、主力市场、投放与真实 1–2 星差评原文，直接对比共同痛点、各自优势与市场空白 |
| `list_waves` | 热门赛道列表（只含已有人跟进的赛道）。默认 15 条紧凑行，按「近 3 月新进且赚到钱的家数」排；支持 `q` 语义+关键词混合搜（按相关度排）、`category`、`store`、`detail=true` 看分析、`offset` 翻页 |
| `get_wave` | 一条赛道的完整档案：定位、分析、逐月进场节奏、全部赚钱成员（标出小团队 / 在投广告数 / 有调查稿）和未过线名单；传任意 App key/链接/名字可反查它所在赛道与进场顺位 |
| `list_early_apps` | 早鸟新苗子列表：近期上架已开始赚钱、全库还没见到同款的 0 跟进新产品。支持 `q` 语义+关键词搜、`category` 品类筛、`store` 平台筛、`min_rev`、按月入/下载/新增评分/首发时间排序 |
| `search_apps` | 在约 52 万个 App 里按一句话或名字模糊搜，按相关度排，自动带出相关赛道；可叠收入、上架时间、小团队、品类等条件 |
| `list_apps` | 按条件筛一批 App，按收入 / 增长 / 新旧排序，开头给总数，支持 `pattern` 全文正则与翻页 |
| `get_app` | 一个 App 的全维档案：默认精简（月入、下载、增长、评分、支持语言/内购区间/官网、所在赛道、四平台投放状态、调查稿摘要）；`sections` 按需加 `reviews`（真实评论与 1–2 星差评痛点）、`charts`（多国榜单历史）、`release_notes`（版本更新日志）、`ads`（四平台逐条广告素材与欧盟触达）、`research`、`description`、`developer`、`daily`、`wave`、`all`；`check_ads=true` 可当场实时查 Apple + Meta 广告 |
| `query_sql` | 对 App 库跑只读 DuckDB SQL，算分布、分位数和跨品类统计 |
| `feed` | 市场动态：每天突然异动的小产品及原因调查。不带参数给最新一天，`day` 看某天，`q` 搜历史，`slug` 读完整调查 |
| `plays` | 打法库：归纳好的起量/冷启动/变现打法（清单、`id` 读全文、`q` 搜），并支持 `case_q` 按获客做法搜数千个真实实证案例、`case_key` 读单品获客细节 |

#### 限额

测试阶段免费（免绑卡，支持 Google 登录与海外手机号），全部 11 个工具已标注 `readOnlyHint: true`（Codex 等客户端无需每次弹窗确认）。

#### 命令行（可选）

一个 Python 文件，只用标准库：

```bash
curl -fsSL https://raw.githubusercontent.com/komako-workshop/stringers/main/cli/stringer -o ~/.local/bin/stringer && chmod +x ~/.local/bin/stringer
export STRINGER_KEY=wt_…
stringer waves
stringer waves --solo
stringer search "按超市价格排一周晚饭"
stringer apps --key https://apps.apple.com/us/app/…/id6759298323
stringer feed
stringer plays
```

### 仓库里有什么

这个仓库是接入层：说明、Claude Code 插件、skill、命令行。采集和服务端不在这里。

[`skills/stringers/`](skills/stringers/SKILL.md) 是一个统一的 skill，按你的问题只加载需要的那一半：做界面读 [`references/design.md`](skills/stringers/references/design.md)，查市场读 [`references/market.md`](skills/stringers/references/market.md)。没接 MCP 时设计这一半照常用。

### 许可

本仓库代码 MIT。网站上的文章、判断和数据版权归「什么值得做」所有，引用请注明出处并附链接。
