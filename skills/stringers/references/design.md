# Mobile UI Design Track (`stringers.io/styles/`)

Give any mobile app (SwiftUI, Expo / React Native, Flutter) an intentional, non-AI-slop visual identity backed by real iOS-verified `DESIGN.md` style cards.

## Step 0 — Classify the task before doing anything

Inspect the workspace first (`DESIGN.md`, existing views/screens, theme files).

| Workspace & request | Mode | What to do |
|---|---|---|
| `DESIGN.md` exists AND user is NOT asking to redesign/reskin | **Continue** | Follow `DESIGN.md` + existing tokens silently. Skip Step 1. |
| Existing app screens already establish a coherent style (even without `DESIGN.md`), AND user asks for a small tweak or to add a screen | **Extend / Refine** | Inherit the existing visual world silently. **Never** pop a style picker. Skip Step 1. |
| Greenfield mobile app/screen (no `DESIGN.md`, no real screens yet) | **New (0→1)** | Run **Step 1 (Visual Pick)** once. |
| User explicitly says the UI looks ugly / AI-generated / asks to reskin, OR explicitly asks `stringers` to pick/redesign the style | **Redesign** | Freeze all existing features, navigation, and user data (never delete real functionality!). Run **Step 1 (Visual Pick)** once. |

## Step 1 — Visual Pick (once per project)

Do **not** ask "would you like to pick a style?" as an empty extra turn. Go straight to delivering value:

1. Fetch the curated 28-style flagship index (single tiny request, ~7KB):
   ```bash
   curl -s https://stringers.io/styles/featured-index.tsv
   ```
   *(Columns: `rank`, `slug`, `name`, `mode`, `categories`, `tagline`, `fits`. All 28 entries are hand-curated flagship styles verified to reproduce reliably. The other 176 styles of the 204-style library live in the full gallery — the user browses those, you don't need to fetch them.)*
2. Pick **3 candidates that sit as far apart from each other as possible** while each still suits the app. In our human pick tests, people repeatedly chose a direction nobody would have predicted from the app as it stood — a light editorial style for a dark social app, a bold dark instrument style for a light habit tracker. Three near-neighbours waste the user's one look at real alternatives, so spread the picks:
   - **#1 — the most natural, high-craft fit** for this app's domain and content shape (trackers/dashboards, reading/notes, cover shelves…).
   - **#2 — a comfortable everyday style in the opposite `mode` from #1**, so the comparison always contains at least one `light` and one `dark` style.
   - **#3 — the bold / unexpected one:** a direction the user would not have asked for, from a different `categories` family than #1 and #2 (e.g. `editorial-print` vs `dark-instrument` vs `bold-graphic` / `playful-illustrated`).
   - **The existing app's color mode is not a preference.** In Redesign mode, never return three styles in the mode the app already has; at least one pick must cross over. Narrow to a single mode only when the user says so in words (e.g. "dark only"), and then push the contrast on categories and temperament instead.
   - Contrast lives in look and mood, never in fitness: every pick must still suit the app's content shape — don't offer a style whose layout fights the product.
3. Present ONE concise message that puts **two links side by side at the top** — the 3-style comparison and the full gallery. The gallery link is a first-class option on its own line, not a footnote: many users find their favourite by browsing.
   - Chinese users:
     - 3-style comparison: `https://stringers.io/styles/?pick=<slug1>,<slug2>,<slug3>`
     - Full gallery (all 204 styles): `https://stringers.io/styles/`
   - English users:
     - 3-style comparison: `https://stringers.io/en/styles/?pick=<slug1>,<slug2>,<slug3>`
     - Full gallery (all 204 styles): `https://stringers.io/en/styles/`
   - Message shape (write it in the user's language, using the matching pair of links above):
     ```
     Side by side, 4 real iOS screens each: https://stringers.io/en/styles/?pick=<slug1>,<slug2>,<slug3>
     Or browse all 204 styles yourself: https://stringers.io/en/styles/ — send me any style name or slug you like.

     1. <Name> (light) — <the vibe, and why it fits this app>
     2. <Name> (dark) — <…>
     3. <Name> (dark · the bold one) — <…>

     Reply 1 / 2 / 3, or any style name/slug from the gallery (plus any color or tweak you want), or `skip` to build with clean native defaults.
     ```
   - If the user sends back a style from the gallery, use it exactly like a pick. A slug is the style name lowercased and hyphenated (`Oat Margin` → `oat-margin`); if that card is not found in Step 2, ask for the slug shown in the gallery.
4. **Unattended / benchmark runs** (when no human can reply): state the 3 picks + `?pick=` URL + full gallery URL, auto-select #1, and continue directly to Step 2.

## Step 2 — Lock `DESIGN.md` + View 4-Screen Reference Strip

The card (`DESIGN.md`) is the exact numerical spec, and the 4-screen reference image gives visual calibration for proportion, density, and visual weight. Once a style (`<slug>`) is locked:

1. Fetch ONLY that style's card into the project root:
   ```bash
   curl -s https://stringers.io/styles/cards/<slug>.md -o DESIGN.md
   ```
   Read it in full before writing code.
2. **View the 4-screen reference image (default whenever you can inspect images):**
   ```bash
   curl -s https://stringers.io/styles/refs/<slug>.jpg -o /tmp/stringers-ref-<slug>.jpg
   ```
   Open and view `/tmp/stringers-ref-<slug>.jpg` before coding to calibrate visual weight, spacing density, and component proportions.
   - Exact colors, font sizes, radii, and spacing always come from `DESIGN.md`, not from eyeballing the image.
   - The image's subject matter, copy, and brand belong to the reference app — never copy its content.
   - If your runtime cannot view images, skip this step gracefully; `DESIGN.md` alone is verified to work.
3. If the user asked for any customization (e.g. "swap the amber accent for espresso brown") or you resolved a conflict between the card and the product's needs (e.g. adding tabs), record a short `## Project overrides` block **immediately after the YAML front matter (`---`)** at the top of `DESIGN.md` — **the user's brief always wins**.
4. Create **one centralized theme/tokens file** (`Theme.swift`, `src/theme/tokens.ts`, etc.) from `DESIGN.md` before writing screen code.

## Step 3 — Build with Craft Floor

1. Read [craft-floor.md](craft-floor.md) silently before editing UI code (never print the checklist to the user).
2. Read ONLY the matching section(s) of [mobile-stacks.md](mobile-stacks.md):
   - SwiftUI → SwiftUI section
   - Expo / React Native → Expo / RN section
   - Chinese / Japanese / Korean UI → CJK Typography section
   - Paywall / Pricing screen → Paywall section
3. In **Redesign** mode: preserve every existing feature, state, control, and data field — replace the visual layer cleanly rather than stripping the product down into a mock.

## Step 4 — Visual Self-Check (when preview is available)

- If an iOS Simulator, Android Emulator, or mobile viewport preview is available in this environment: build, capture screenshot(s), **open and inspect them visually** against `DESIGN.md` + `craft-floor.md`, fix every defect you spot, and re-shoot once.
- If no simulator/preview is available: run a quick static check on tokens, contrast, safe areas, and the card's `Never` list — do not block or fail because of missing simulator tooling.
