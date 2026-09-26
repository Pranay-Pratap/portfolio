# Portfolio site improvements — design

Date: 2026-09-26
Scope: `index.html` (single file, no build step, no new runtime dependencies)

## 1. Polish & correctness
- Head: Open Graph + Twitter card tags, `theme-color`, canonical, inline SVG favicon (pixel plane).
- Share image: `og.png` (1200×630) — split-flap "PRANAY" on the night sky, committed next to `index.html`.
- Accessibility:
  - Flap name wrapped in `<h1>` containing a `<button>` (keyboard operable). Visible flaps are `aria-hidden`; a visually-hidden "Pranay" is the accessible name.
  - Quote rotator gets `aria-live="polite"`.
- Hover lift only on interactive tiles (charter, boarding pass). Other tiles: border highlight only.
- Fixes: remove safe-area padding from `:root`; pause clock while `document.hidden`; beacon anchored to the tower image so it stays aligned at all widths.

## 2. Client-winning content
- Departures rows gain a one-line description per product:
  - Floo — find Aadhaar-verified travel companions for trips in India. iOS / Android.
  - Murmur — a floating dot for macOS that turns your voice into Apple Notes, on-device. Free.
- Charter tile: 3-step flight plan (Brief → Build → Launch) and a "you get" line (source code, deployment, handover). CTAs: "Request a quote" (mailto with brief template in body) and "See the work" (anchor to Departures).
- No invented metrics, prices, or testimonials.

## 3. Visual features
1. Expandable flight rows: each live row is a `<details>`/`<summary>`; the panel shows description, platform, stack, and a "Visit ↗" link. Works without JS.
2. Boarding pass flip: a "flip ↻" button rotates the pass (3D, `rotateY`); back face shows the charter process + CTA. Front keeps contact links. Reduced motion: instant swap, no rotation.
3. Radar sweep in the Ground control tile: CSS conic-gradient sweep with FL·01 / MU·02 blips. Decorative (`aria-hidden`).
4. Status flip-board: status cells cycle with split-flap animation between truthful values — Floo: IN FLIGHT ↔ iOS·ANDROID; Murmur: IN FLIGHT ↔ FREE. Next departure stays SCHEDULED. Stopped under reduced motion.

All new animation disabled under `prefers-reduced-motion`. All features must work in night and dawn themes and at 375px width.

## Testing
- Headless browser screenshots: desktop (1280) and mobile (375), night and dawn.
- Keyboard-only pass: flap button, row expand, pass flip, all links reachable.
- Reduced-motion emulation.
- HTML validation (vnu or equivalent) with no errors.
