---
name: stringers
description: "Stringers (stringers.io): the indie mobile app builder's toolbox. (1) Mobile UI design, zero login: 204 real iOS-verified DESIGN.md styles with a live ?pick= side-by-side preview, used to build or redesign SwiftUI / Expo / React Native / Flutter screens without AI slop and without deleting features. (2) App market intelligence via the Stringers MCP: revenue, ads, niches and growth playbooks across 520k apps. Use when designing a new mobile app UI, redesigning an ugly or AI-looking screen, picking an app visual style, or researching profitable app niches and competitors. Triggers: redesign this screen with stringers, design this app screen, pick a UI style, make it look less AI-generated, find profitable app niches, competitor revenue. 触发词：用 stringers 帮我设计这个界面、做个 App 界面、挑 UI 风格、去 AI 味、界面换皮、找赚钱赛道、查竞品收入。"
version: 1.0.0
user-invocable: true
homepage: https://stringers.io
---

# Stringers

One toolbox for indie mobile app builders — combining **visual design craft** (works immediately with zero login) and **app market intelligence** (when the `stringer` MCP is connected).

## Routing (load ONLY the track you need)

| User intent | Load | Notes |
|---|---|---|
| Building a new mobile app/screen, redesigning an ugly or AI-looking UI, choosing a visual style, or explicitly asking `stringers` to style/polish UI | [references/design.md](references/design.md) | Works with zero login: 204 real iOS styles at `https://stringers.io/styles/`. |
| Finding profitable app niches, early-bird apps, competitor revenue/ads, or growth playbooks | [references/market.md](references/market.md) | Uses the `stringer` MCP if connected; otherwise tells the user in one line how to connect at `stringers.io`. |
| Both (e.g. "find a niche and build its first screen") | Load `market.md` first, then `design.md` | Keep research and UI cleanly separated in steps. |

## Non-negotiable restraint

- **Never interrupt routine edits.** If the project already has screens or a `DESIGN.md` and the user asks for a small tweak or a new screen inside that existing app (e.g. "make the add button bigger", "add a settings screen"), inherit the existing visual style silently. Do **not** launch a style picker unless the user explicitly asks to redesign/reskin or switch styles.
- **Mobile apps only for the design track.** If the user is building a desktop web marketing site, say so in one line and don't force mobile styles onto it.
