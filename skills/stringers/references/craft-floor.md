# Mobile Craft Floor

Read this silently before writing or editing UI code. Never announce or recite this checklist in your reply.

> **Precedence rule:** The user's explicit brief and the committed `DESIGN.md` always override the defaults below — except items explicitly marked **Hard ban**. Model laziness or habit never overrides anything here.

## Verify (mechanical craft checks)

1. **Product over skin.** Layout follows the product's real structure (a list stays scannable, a form stays clear, a dashboard stays legible). Never drop real features or invent fake stats/badges just to make a screenshot look fuller.
2. **Token discipline.** Pull every color, surface, radius, and type style from the centralized theme file derived from `DESIGN.md` — no ad-hoc hexes scattered across views. Support both light and dark coherently when the app supports both appearances.
3. **Mobile boundaries.** Respect safe areas for headers, tab bars, bottom CTAs (`.safeAreaInset` / `useSafeAreaInsets`), and keyboard avoidance. Ensure scrollable content never hides behind pinned bars. Touch targets ≥ 44×44pt (iOS) / ≥ 48×48dp (Android). Text must never clip, overlap, or break mid-word.
4. **Contrast & crisp icons.** Body and secondary text ≥ 4.5:1 contrast; large text and meaningful icons ≥ 3:1 (when `DESIGN.md` specifies exact muted/tertiary ink or accent colors, follow `DESIGN.md` for those roles unless it violates a Hard ban). Use one consistent vector icon family (SF Symbols / `expo-symbols` / Lucide) with uniform weight — **never use emoji as UI icons**.
5. **Real states & copy restraint.** Include plausible realistic data (no "Lorem ipsum" or sterile round numbers), clear press feedback without layout jitter, and clean empty/loading states. Keep subtitles and captions crisp (prefer 1 line over 2) and drop redundant explanatory helper footers so typography and whitespace breathe.

## Refuse (AI-slop habits)

**Hard bans (never ship):**
- Nested cards (cards inside cards).
- White text smaller than 17pt on bright/saturated fills.
- Decorative crown / diamond / sparkle clichés to signal "Pro / Premium".
- Fake urgency countdowns, invented ratings/user counts, or hidden pricing on paywalls.

**Lazy defaults (refuse unless the user brief or `DESIGN.md` explicitly calls for them):**
- Generic blue→purple decorative gradients.
- Putting a colored rounded-square icon tile on every single list row.
- Repeating identical icon + title + subtitle SaaS cards as the whole mobile screen.
- Decorative progress rings, sparklines, or "hero metric" boxes standing in for actual content.
- Gradient text or frosted-glass blur slapped on as decoration.
- Using one identical corner radius on every container, chip, and button.
- A kicker/eyebrow micro-label stacked above every heading.

## Compass

The floor guards craft; `DESIGN.md` owns taste. Once the mechanical checks are clean, **when torn between playing it safe and committing to the chosen `DESIGN.md`, commit to the style.**
