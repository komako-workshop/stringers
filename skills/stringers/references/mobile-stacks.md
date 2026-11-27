# Mobile Stack Notes (read ONLY the section(s) you need)

## SwiftUI (iOS 18–26)

- **System chrome first**: Use native `NavigationStack`, `TabView`, `.sheet`, `.searchable`, and `.safeAreaInset(edge: .bottom)` for pinned primary actions (with matching scroll bottom padding). Let `DESIGN.md` tint the chrome rather than rebuilding brittle fake nav bars unless the style explicitly calls for custom chrome.
- **Modern APIs**: Use `@Observable` (Observation framework), `ForegroundStyle`, semantic colors in `Theme.swift`, `monospacedDigit()` on live counters/prices/timers, and `SF Symbols` with consistent `.fontWeight` / `.symbolRenderingMode`.
- **iOS 26 Liquid Glass**: System bars, toolbars, and sheets get Liquid Glass automatically — never stack fake CSS-style blur cards behind system chrome.
- **No `GeometryReader` abuse**: Prefer `VStack`/`HStack`/`Grid`/`.containerRelativeFrame` over `GeometryReader` for everyday layout.

## Expo / React Native (SDK 54–58)

- **Version gate**: Always read `package.json` first and match the installed Expo / RN / Expo Router APIs instead of guessing or upgrading packages.
- **Native feel, not web-in-a-wrapper**:
  - Centralize colors, spacing, radii, and typography in `src/theme/tokens.ts` (use `StyleSheet.create`, avoid scattered inline hex styles).
  - Safe areas: use `useSafeAreaInsets()` from `react-native-safe-area-context` (not deprecated core `SafeAreaView` or hard-coded notch heights).
  - Navigation & chrome: prefer `expo-router` native stack / tabs (`expo-router/native-tabs` on SDK 58) and real native sheets/modals over `useState` fake overlays.
  - Icons: `expo-symbols` (`SymbolView`) or `@expo/vector-icons` / `lucide-react-native` — one consistent stroke family across the app, never emoji.
  - Press states: use `Pressable` with subtle opacity/scale/background feedback (no layout-shifting press jumps) and `hitSlop` so every tap target is ≥ 44×44pt.
  - Numerals: use `fontVariant: ['tabular-nums']` on prices, timers, and counters; `fontFamily: 'ui-serif'` / `'ui-rounded'` on iOS where the style calls for serif/rounded display voice (with clean Android fallback).
  - Hairlines: use `StyleSheet.hairlineWidth` for crisp 1px dividers rather than heavy `1` or `2` borders.

## CJK Typography (Chinese / Japanese / Korean)

- SF Rounded, SF Mono, compressed/expanded widths, and Latin display fonts do not contain CJK glyphs — Chinese falls back to PingFang SC.
- Express the card's display voice through **numerals and Latin** where present, and translate the voice into CJK using **clear weight & size contrast** in PingFang SC (e.g. Heavy vs Regular, generous line-height, restrained letter spacing).
- Never leave an orphan 1–2 CJK characters alone on the last line of a headline or subtitle — shorten the phrasing or split cleanly across lines. Keep titles ≤ 2 lines.

## Paywall & Subscription Screens

- Wear the exact same `DESIGN.md` skin as the rest of the app (never drop a jarring forced-light template into a dark app).
- Make the actual charged price the largest, clearest number; place trial terms and renewal cadence right next to the primary CTA; keep Close and Restore Purchases clearly visible.
- Never fabricate ratings, user counts, or fake countdown timers.
