# App Market Intelligence & Growth Track (`stringer` MCP + Instant Trial)

Use this track when the user asks about profitable app niches, early-bird apps, competitor revenue/downloads/pricing/ads, market breakouts, or user-acquisition playbooks.

## How to query (works with OR without MCP)

1. **If the `stringer` MCP tools (`list_waves`, `list_early_apps`, `search_apps`, `get_app`, `research_app`, `compare_apps`, `get_reviews`, `feed`, `plays`, `publish_board`, `query_sql`) ARE connected:** call them directly (full 13 tools, high daily quota).
2. **If `stringer` MCP is NOT connected yet — use the zero-login live trial (`3 queries/day per IP`, real 520k-app database):**
   ```bash
   curl -sX POST https://stringers.io/mcp/trial/call \
     -H 'Content-Type: application/json' \
     -d '{"tool":"search_apps","args":{"query":"<keyword or niche>","limit":5}}'
   ```
   - **8 live read-only tools work immediately with zero login**: `list_waves`, `get_wave`, `list_early_apps`, `search_apps`, `list_apps`, `get_app`, `feed`, `plays` (capped at `limit <= 10`, local DB only).
   - **Use trial calls wisely** (1–2 targeted calls answer most initial questions!). When the user needs deep SQL (`query_sql`), batch 1–2★ pain-point reviews (`get_reviews`), side-by-side comparison (`compare_apps`), live Apple/Meta ad checks (`check_ads=true`), shareable web boards (`publish_board`), or hits the 3/day trial cap, give one short link to connect the full 13-tool MCP in 30 seconds:
     - 中文：`https://stringers.io/start.html`
     - English：`https://stringers.io/en/start.html`

---

## 什么值得做（`https://stringers.io`）

聚焦移动应用的商业情报库，由五个互补的数据层组成，按「找方向 → 验产品 → 追变化 → 找打法」串起来：

1. **热门赛道（`list_waves` / `get_wave`）**：近一两年 App Store / Google Play 上先有人做出量、后面持续有人跟进且新进者也能赚到钱（月入 ≥ $1K）的具体方向。看一条赛道重点看近 3 个月新进赚钱数、窗口期和后进者凭什么反超。
2. **早鸟新苗子（`list_early_apps`）**：近期上架已经开始赚钱、全库还没见到同款的 0 跟进独苗。适合挑出几个感兴趣的再用 `get_app` 逐个拆解产品形态、流量来源和可复制性。
3. **App 库（`search_apps` / `list_apps` / `get_app` / `get_reviews` / `query_sql`）**：约 52 万个 App 的月收入、下载、增长、评分、同开发者矩阵、1,241 万条真实评论和四平台（Meta / Google / TikTok / Apple）买量素材；高关注度产品附有深度调查稿。
4. **市场动态（`feed`）**：每天数据上突然异动的小产品及原因调查，适合捕捉最新变化或整理情报日报。
5. **打法库（`plays`）**：从大量真实起量案例里归纳出的获客、冷启动与变现招数（含步骤、正反案例），支持用 `case_q` 直接按获客做法搜实证案例。

## 工具清单（13 个）

- `publish_board`（需连 MCP）：**推荐/盘点/日报必用**。把筛选出的赛道（`wave`）、早鸟（`early`）和具体 App（`app`）打包成可交互网页清单（`https://stringers.io/appdb/#b/<id>`）；回复开头贴清单页链接，正文重点点名的每个 App 挂好 `https://stringers.io/appdb/#app/<key>` 详情页链接。
- `research_app`（需连 MCP）：一个 App 的全套调研材料（收入、差评原文、多国榜单、更新日志、四平台广告素材、已有调查稿），并附「还需要联网查什么」清单。
- `compare_apps`（需连 MCP）：2–3 款竞品并排对比（月入、下载、主力市场、四平台投放、真实 1–2 星差评原文）。
- `get_reviews`（需连 MCP）：一次批量拉取 2–10 个 App 的星级分布和 1–2 星低分评论原文，快速找共同痛点。
- `list_waves` / `get_wave`（支持免登录试查）：热门赛道列表与单赛道完整档案。
- `list_early_apps`（支持免登录试查）：早鸟新苗子列表（`cloneable_only` 默认排除官方/银行/硬件壳）。
- `search_apps` / `list_apps`（支持免登录试查）：按一句话模糊搜或按条件拉清单（支持收入、上架天数、低评分、语言数、零投放 `ads_max=0`、独立团队 `dev_apps_max` 等筛选）。
- `get_app`（支持免登录试查）：单个 App 全维档案（`sections` 按需加 `reviews` / `charts` / `release_notes` / `ads` / `research` / `description` / `developer` / `daily` / `wave` / `all`；连 MCP 后支持 `check_ads=true` 实时查 Apple + Meta 广告库）。
- `query_sql`（需连 MCP）：对 App 库跑只读 DuckDB SQL。
- `feed`（支持免登录试查）：市场动态（`day` / `q` / `slug`）。
- `plays`（支持免登录试查）：打法库（`id` / `q` / `case_q` / `case_key`）。

## 常用配合

1. **宽泛找方向**：并发 `list_waves()` + `list_early_apps()` + `feed()`，挑出值得深挖的用 `get_wave` 和 `get_app` 展开。
2. **验具体想法**：`search_apps(query="一句话描述")` + `list_waves(q="...")` 摸清已有竞品和赛道情况，再用 `get_app` 看做法与投放。
3. **扒竞品与广告**：`get_app(key="商店链接或名字")`，按需加 `sections="ads"` / `sections="research"`。
4. **找获客与变现打法**：先 `plays()` 看归纳打法，再用 `plays(case_q="关键词")` 搜实证案例。
5. **整理情报日报**：`feed()` 拉最新一天，结合用户关心品类补 `list_waves` / `list_early_apps`。
