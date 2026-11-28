---
name: stringers
description: "Stringers (stringers.io): the indie mobile app builder's toolbox — both tracks work immediately with zero login. (1) Mobile UI design: 204 real iOS visual style specs (DESIGN.md + 4-screen strips + live ?pick= comparison) + strict anti-AI-slop mobile craft floor for SwiftUI / Expo / React Native / Flutter. (2) App market intelligence across 520k apps: 3 free live trial queries/day via HTTP with zero login, scaling to the full 13-tool Stringers MCP (revenue, niches, early birds, 12.4M reviews, 4-platform ads, SQL, shareable boards). Use when designing or polishing mobile app UI, picking a visual style, validating profitable app niches, or checking competitor revenue/pricing/ads. Triggers: redesign this screen with stringers, design this app screen, pick a UI style, make it look less AI-generated, find profitable app niches, competitor revenue. 触发词：用 stringers 帮我设计这个界面、做个 App 界面、挑 UI 风格、去 AI 味、界面换皮、找赚钱赛道、查竞品收入。"
version: 1.1.0
user-invocable: true
homepage: https://stringers.io
---

# Stringers

One toolbox for indie mobile app builders — pairing **mobile visual craft** (numerical `DESIGN.md` tokens + anti-AI-slop craft floor) with **live app market intelligence** (520k apps' revenue, niches, ads, and growth playbooks). **Both tracks work immediately with zero login.**

## Routing (load ONLY the track you need)

| User intent | Load | Notes |
|---|---|---|
| Building a new mobile app/screen, redesigning an ugly or AI-looking UI, choosing a visual style, or explicitly asking `stringers` to style/polish UI | [references/design.md](references/design.md) | Works zero-login: 204 real iOS visual styles at `https://stringers.io/styles/` (focuses on visual system & craft while keeping your product's own IA and features). |
| Finding profitable app niches, early-bird apps, competitor revenue/pricing/ads, or growth playbooks | [references/market.md](references/market.md) | Uses the `stringer` MCP if connected (full 13 tools); **without MCP, works immediately via 3/day zero-login live trial (`POST /mcp/trial/call`)** against the real 520k-app database. |
| Both (e.g. "find a niche and build its first screen", or building a paywall/new app with real competitor pricing benchmarks) | Load `market.md` first, then `design.md` | Pull live revenue & pricing reality first, then lock visual craft cleanly. |

## Non-negotiable restraint

- **Never interrupt routine edits.** If the project already has screens or a `DESIGN.md` and the user asks for a small tweak or a new screen inside that existing app (e.g. "make the add button bigger", "add a settings screen"), inherit the existing visual style silently. Do **not** launch a style picker unless the user explicitly asks to redesign/reskin or switch styles.
- **Clear scope on the design track.** The design track owns visual tokens, typography, mobile spacing, and anti-AI-slop craft for **mobile apps** — it intentionally preserves the user's product structure and features rather than rewriting product architecture. For desktop web marketing sites, say so in one line and don't force mobile styles onto them.
