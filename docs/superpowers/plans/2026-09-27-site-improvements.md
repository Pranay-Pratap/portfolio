# Site Improvements Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Implement `docs/superpowers/specs/2026-09-26-site-improvements-design.md` in `index.html`.

**Architecture:** Everything stays in the single `index.html` (inline CSS + one IIFE script). New assets: `og.png` (share image) and `.vercelignore` (keep `docs/` off the public site). Behaviour is verified by a Playwright script run against the local file with system Chrome.

**Tech Stack:** HTML/CSS/vanilla JS; Playwright (`channel: "chrome"`) and `html-validate` for checks, installed outside the repo.

**Site URL:** `https://portfolio-beta-lemon-3d57ut9saa.vercel.app/` (Vercel).

**Deviations from spec (decided while planning):**
- The 3-step Brief → Build → Launch process lives only on the back of the boarding pass. The charter tile gets the "you get" line and CTAs instead, so the same content isn't shown twice.
- Flight details panels show facts published on each product site (platform, verification/price/privacy), not a tech stack, because per-product stacks aren't published.
- Favicon is a split-flap "P" tile (reads at 16px) instead of the plane sprite (20px tall, non-square).

---

### Task 0: Test harness

**Files:** Create (outside repo): `$SCRATCH/check.mjs`, `$SCRATCH/package.json`

- [ ] `npm i playwright@1.55 html-validate` in the scratch dir.
- [ ] `check.mjs` opens `index.html` via `file://` in Chrome and asserts every requirement below (head tags, h1/button name, aria-live, root padding, hover lift only on interactive tiles, beacon inside tower, `#departures`, row descriptions, mailto body, "See the work", `.youget`, two `details.flight` that open by click and Enter with the right text and link, pass flip toggles `inert` and moves focus, 3 legs on back, radar `aria-hidden` with 2 blips, Floo status reaches `iOS·Android`, reduced motion keeps status + stops radar but flip still works, no console errors, no horizontal scroll at 1280/375 in both themes). `--shots` writes screenshots.
- [ ] Run `node check.mjs` → expect FAIL (baseline).

### Task 1: Head tags, favicon, share image, `.vercelignore`

**Files:** Modify `index.html:4-10`; Create `og.png`, `.vercelignore`

- [ ] Add after `<meta name="description">`:

```html
<link rel="canonical" href="https://portfolio-beta-lemon-3d57ut9saa.vercel.app/">
<meta name="theme-color" content="#090d1c">
<meta property="og:type" content="website">
<meta property="og:url" content="https://portfolio-beta-lemon-3d57ut9saa.vercel.app/">
<meta property="og:title" content="Pranay · Founder, Engineer, One-Man Airline">
<meta property="og:description" content="…same text as meta description…">
<meta property="og:image" content="https://portfolio-beta-lemon-3d57ut9saa.vercel.app/og.png">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:creator" content="@PranayPratap_">
<link rel="icon" href="data:image/svg+xml,…split-flap P…">
```

- [ ] Generate `og.png` (1200×630) by screenshotting a small HTML page (night gradient, stars, six amber flap tiles "PRANAY", subtitle) with Playwright.
- [ ] `.vercelignore` containing `docs`.

### Task 2: Accessibility and polish

**Files:** Modify `index.html` (CSS `:root`, `.tile:hover`, `.t-atc`; identity tile markup; quote wrapper; clock + flap JS)

- [ ] Remove `padding-top`/`padding-bottom` from `:root`.
- [ ] `.tile:hover` keeps only the border colour; `transform: translateY(-2px)` moves to `.t-charter:hover, .t-pass:hover`.
- [ ] Identity tile: `<h1 class="flaph"><button class="flapline" id="flapline" type="button" title="Replay"><span class="sr-only">Pranay</span><span class="flaps" id="flaps" aria-hidden="true"></span></button></h1>`; button reset CSS; `.sr-only` utility; JS builds flaps into `#flaps`, click handler stays on `#flapline`.
- [ ] `#qwrap` gets `aria-live="polite"`.
- [ ] Clock uses start/stop driven by `visibilitychange`.
- [ ] Beacon moves inside `.tower` (`position:absolute; top:4%; left:50%`).

### Task 3: Charter content

- [ ] Replace the CTA with `.ctas` (grid): `a.cta` (mailto with subject + brief template body) and `a.cta.ghost href="#departures"` "See the work". Add `<p class="youget"><b>You get:</b> the source code, the deployment, and a clean handover. No lock-in.</p>` above the availability line.

### Task 4: Expandable departures rows

- [ ] Departures tile gets `id="departures"`. Each live row becomes `<details class="flight"><summary class="drow">…code, name + .desc, route, .st[data-alt], .chev…</summary><div class="fd">…p, dl.fdg, a.fdlink…</div></details>`. Grid gets a 5th 14px column (header and scheduled row get an empty span). The summary marker is hidden, the chevron rotates when `[open]`, `summary:focus-visible` gets the outline, and mobile columns are `1fr 116px 14px`.

### Task 5: Boarding pass flip

- [ ] `.t-pass` wraps `.pcard > .pface.pfront + .pface.pback[inert]`. Faces stack in one grid cell, `backface-visibility:hidden`, back `rotateY(180deg)`, `.t-pass.flipped .pcard` rotates. Paper background and border move from the tile to the faces. Each face has a `.flipbtn` in the stub. JS toggles `.flipped`, swaps `inert`, and focuses the other face's button. Under reduced motion there is no transition.

### Task 6: Radar

- [ ] `<div class="radar" aria-hidden="true"><span class="sweep"></span><span class="blip b1"><span>FL·01</span></span><span class="blip b2"><span>MU·02</span></span></div>` under the ATC paragraph. Conic sweep 4s linear; each blip's fade keyframe has a negative delay so it flashes as the sweep passes (b1 at 130°, delay −3.333s; b2 at 290°, delay −1.556s). Animations are added to the reduced-motion list.

### Task 7: Status flip-board

- [ ] `.st[data-alt]` cells: every 6s (skipped while hidden) scramble to the other value, one cell after another. Store the original text in `data-a`. Skipped entirely under reduced motion.

### Task 8: Verify and commit

- [ ] `node check.mjs --shots` → all PASS; review screenshots (night/dawn × 1280/375).
- [ ] `npx html-validate index.html` → no errors.
- [ ] Commit `index.html`, `og.png`, `.vercelignore`, plan.
