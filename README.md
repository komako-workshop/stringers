# stringer:把「什么值得做」接进你的 agent

[什么值得做](https://stringers.io) 是面向独立开发者和小团队的 **App 商业情报库**，帮你回答两个核心问题：**现在做什么方向有机会；跑出来的产品是怎么拿到用户和赚到钱的。**

整套库由五个互补的数据层组成：

- **热门赛道（`list_waves` / `get_wave`）**：把近一两年 App Store 和 Google Play 上做同一件事的新产品归成四百多条赛道，每条知道谁先做出量、谁第几个跟进来、近 3 个月新进的人里有多少家赚到钱（月入 ≥ $1K）——这是目前大家都在做、确定性相对较高的方向。
- **早鸟新苗子（`list_early_apps`）**：近期上架已经开始赚钱、全库还没见到同款的 0 跟进独苗，适合挑出来逐个拆解产品形态、流量来源和可复制性。
- **52 万 App 全量库（`search_apps` / `list_apps` / `get_app` / `query_sql`）**：App Store 与 Google Play 约 52 万个 App 的月收入、下载、增长、评分、同开发者矩阵、四平台（Meta / Google / TikTok / Apple）买量素材与商店介绍全文，高关注度产品还附有深度调查稿。
- **市场动态（`feed`）**：每天数据上突然异动的小产品及原因调查，适合追踪最新热点、查某个产品为什么这几天火了，或整理成每日情报日报。
- **打法库（`plays`）**：从大量真实起量案例中归纳出的可复用增长、冷启动和变现做法（含步骤、正反案例），并支持按获客做法直接检索实证案例。

网页上是整理好的榜单和调查。接进你自己的 agent（Claude Code、Cursor、Codex、Trae……）之后，它能直接读这五个数据层，再结合**你自己的情况**给出判断。

这个仓库是接入层：说明、Claude Code 插件、skill、命令行。采集和服务端不在这里。

> **English:** Stringers (「什么值得做」, [stringers.io](https://stringers.io)) is an App intelligence MCP server for indie developers and small teams: 400+ app niches (`list_waves` / `get_wave`) showing who started each one and whether recent followers earn; 300+ early-bird apps (`list_early_apps`) earning with no look-alike yet; ~520,000 App Store & Google Play apps with revenue, growth, 4-platform ad creatives and deep research (`search_apps` / `list_apps` / `get_app` / `query_sql`); daily market breakouts (`feed`); and growth playbooks with real cases (`plays`). Remote MCP server: `https://stringers.io/mcp/{token}` (OpenAI `search`/`fetch` connector: `https://stringers.io/mcp/oai/{token}`).

## 30 秒接上

1. 打开 [stringers.io](https://stringers.io)，**登录**（手机号 + 验证码，或 Google 账号；未注册的自动建号）。
2. 点**配置 agent**，复制那段话，发给你的 agent。
3. 它自己把配置加好。接通以后直接问。

你的专属地址长这样：

```
https://stringers.io/mcp/wt_xxxxxxxxxxxxxxxxxxxxxxxx
```

这个地址就是你的 token，别发到公开的地方。泄露了在「配置 agent」里点「换一个新地址」，旧的立刻失效。

## Claude Code 插件

已经有 token 后，在 Claude Code 里：

```
/plugin marketplace add komako-workshop/stringers
/plugin install stringer@stringers
```

安装时会问 token，MCP 和 skill 一起装好。

## 手动配置

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

## 能问什么

- 最近有哪些 App 赛道新进去的人也能赚到钱？哪些早鸟新苗子刚起步还没人跟？
- 做「按预算排一周晚饭」这件事的 App 有多少个？各自月入多少、第几个进场、谁在买量？
- 这个 App（贴商店链接）月入多少、怎么做起来的、在 Meta / Google / TikTok / Apple 投了什么广告素材？
- 今天市场动态里有哪些产品突然涨起来了？帮我整理一份我关心领域的日报。
- 我做了一个工具 App 没人知道，有哪些可照抄的起量打法和实证案例？

## 工具（10 个）

| 工具 | 做什么 |
|---|---|
| `publish_board` | 把筛选出的赛道、早鸟新苗子和具体 App 打包成一张专属可交互网页清单（`https://stringers.io/appdb/#b/<id>`），带图标、月入走势、赛道图标墙、月度进场柱图和 AI 定制点评，一键点进详情页与广告素材 |
| `list_waves` | 热门赛道列表（只含已有人跟进的赛道）。默认 15 条紧凑行，按「近 3 月新进且赚到钱的家数」排；支持 `q` 语义+关键词混合搜（按相关度排）、`category`、`store`、`detail=true` 看分析、`offset` 翻页 |
| `get_wave` | 一条赛道的完整档案：定位、分析、逐月进场节奏、全部赚钱成员（标出小团队 / 在投广告数 / 有调查稿）和未过线名单；传任意 App key/链接/名字可反查它所在赛道与进场顺位 |
| `list_early_apps` | 早鸟新苗子列表：近期上架已开始赚钱、全库还没见到同款的 0 跟进新产品。支持 `q` 语义+关键词搜、`category` 品类筛、`store` 平台筛、`min_rev`、按月入/下载/新增评分/首发时间排序 |
| `search_apps` | 在约 52 万个 App 里按一句话或名字模糊搜，按相关度排，自动带出相关赛道；可叠收入、上架时间、小团队、品类等条件 |
| `list_apps` | 按条件筛一批 App，按收入 / 增长 / 新旧排序，开头给总数，支持 `pattern` 全文正则与翻页 |
| `get_app` | 一个 App 的档案：默认精简（月入、下载、增长、评分、所在赛道、四平台投放状态、调查稿摘要）；`sections` 按需加 `ads`（四平台逐条广告素材与欧盟触达）、`research`（调查稿全文）、`description`（商店介绍）、`developer`、`daily`、`wave`、`all` |
| `query_sql` | 对 App 库跑只读 DuckDB SQL，算分布、分位数和跨品类统计 |
| `feed` | 市场动态：每天突然异动的小产品及原因调查。不带参数给最新一天，`day` 看某天，`q` 搜历史，`slug` 读完整调查 |
| `plays` | 打法库：归纳好的起量/冷启动/变现打法（清单、`id` 读全文、`q` 搜），并支持 `case_q` 按获客做法搜数千个真实实证案例、`case_key` 读单品获客细节 |

## 限额

测试阶段免费，所有 9 个工具均为只读（已标注 `readOnlyHint: true`，Codex 等客户端无需每次弹窗确认）。

## 命令行（可选）

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

## skill（可选）

[`skills/stringer/SKILL.md`](skills/stringer/SKILL.md) 提供了完整的 Big Picture 导览与常用配合方法。接了 MCP 本身就已内建相同的导览说明；想进一步固化习惯可以顺手装上。

## 许可

本仓库代码 MIT。网站上的文章、判断和数据版权归「什么值得做」所有，引用请注明出处并附链接。
