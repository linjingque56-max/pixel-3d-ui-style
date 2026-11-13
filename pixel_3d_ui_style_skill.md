---
name: pixel-3d-ui-style
description: Design and build interactive HTML web apps, games, dashboards, and simulations using the tactile 3D beveled pixel-art aesthetic (MoneyTown style). Use whenever the user asks for pixel 3D style (像素立体风), retro arcade/handheld simulation UI, isometric pixel scenes, beveled pixel buttons, or references the MoneyTown visual system.
---
# Pixel 3D UI Style

Build self-contained interactive HTML applications with a tactile 3D beveled pixel-art visual system, combining retro arcade bevels, isometric 2.5D pixel-art canvas worlds, smooth camera zoom transitions, and contextual UI highlighting.

## When to Use

- The user asks to apply the pixel 3D style (像素立体风), MoneyTown style, or retro simulation game aesthetic to a web app, game, or dashboard.
- The user wants tactile 3D beveled pixel buttons, pixel-bordered cards, isometric 2.5D block scenes, or smooth bird's-eye-to-storefront camera transitions.
- The user asks to package or reuse the visual language, layout structure, and dynamic animations from the MoneyTown project.

## Steps

### 1. Configure Design Tokens, Fonts, and Starfield Theme

Include the `Press Start 2P` and `DotGothic16` Google Fonts and define the dual-theme CSS variables (daylight sky gradient by default, starry deep-indigo night in dark mode):

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=DotGothic16&family=Press+Start+2P&display=swap">
<style>
:root {
  --bg: #9fdcff;
  --bg2: #e4f6ff;
  --pn: #fff1cf;
  --ink: #2a2140;
  --tx: #2a2140;
  --hi: rgba(255, 255, 255, .65);
  --lo: rgba(42, 33, 64, .2);
  --gold: #ffb800;
  --teal: #14a56e;
  --pink: #ff5d8f;
  --blue: #3b82f6;
  --so: 0;
  --px: 'Press Start 2P', monospace;
  --bd: 'DotGothic16', 'PingFang SC', 'Microsoft YaHei', system-ui, sans-serif;
}
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --bg: #16132f; --bg2: #2b2560; --pn: #2d2858;
    --ink: #07051a; --tx: #f4f1ff;
    --hi: rgba(255, 255, 255, .2); --lo: rgba(0, 0, 0, .35);
    --gold: #ffd23f; --teal: #3ddc97; --so: 1;
  }
}
*, *::before, *::after { box-sizing: border-box; }
body {
  margin: 0; min-height: 100vh;
  background: linear-gradient(var(--bg), var(--bg2));
  color: var(--tx); font: 16px/1.6 var(--bd); overflow-x: hidden;
}
canvas { image-rendering: pixelated; image-rendering: crisp-edges; }
#stars { position: fixed; left: 0; top: 0; width: 2px; height: 2px; opacity: var(--so); pointer-events: none; }
</style>
```

Initialize the procedural starfield on load:

```javascript
document.querySelector('#stars').style.boxShadow = Array.from(
  { length: 60 },
  () => `${Math.random() * 100}vw ${Math.random() * 100}vh 0 #fff`
).join(',');
```

### 2. Style 3D Beveled Pixel Containers and Tactile Buttons

Use 4px solid ink borders, dual inner bevels (top-left highlight, bottom-right shadow), and hard vertical drop shadows to give every panel and button physical thickness:

```css
/* 3D Beveled Panel */
.px {
  background: var(--pn);
  border: 4px solid var(--ink);
  box-shadow:
    inset 4px 4px 0 var(--hi),
    inset -4px -4px 0 var(--lo),
    0 6px 0 var(--ink);
}

/* Tactile Arcade Button */
.btn {
  font: 10px/1.4 var(--px);
  padding: 10px 12px;
  background: var(--gold);
  color: #2a2140;
  border: 4px solid var(--ink);
  cursor: pointer;
  white-space: nowrap;
  box-shadow:
    inset 3px 3px 0 rgba(255, 255, 255, .55),
    inset -3px -3px 0 rgba(0, 0, 0, .2),
    0 4px 0 var(--ink);
  transition: transform .12s, box-shadow .18s, background-color .18s, opacity .18s;
}
.btn:active:not(:disabled) {
  transform: translateY(3px);
  box-shadow:
    inset 3px 3px 0 rgba(255, 255, 255, .55),
    inset -3px -3px 0 rgba(0, 0, 0, .2),
    0 1px 0 var(--ink);
}
.btn.g { background: var(--pn); color: var(--tx); }
.btn.on { background: var(--pink); color: #fff; }
.btn.teal { background: var(--teal); color: #fff; }
.btn.s { font-size: 9px; padding: 7px 9px; }
.btn:disabled { opacity: .36; cursor: not-allowed; filter: grayscale(.35); }

/* Contextual Action Highlighting Pulse */
.btn.act-hi:not(:disabled) {
  box-shadow:
    0 0 0 3px var(--gold),
    0 0 14px var(--gold),
    inset 3px 3px 0 rgba(255, 255, 255, .7),
    0 4px 0 var(--ink);
  animation: actPulse 1.15s ease-in-out infinite;
}
.btn.act-hi.primary-hi:not(:disabled) {
  background: var(--pink); color: #fff;
  box-shadow: 0 0 0 3px #fff, 0 0 18px var(--pink), 0 4px 0 var(--ink);
}
@keyframes actPulse {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-2px); }
}

/* 3D Extruded Pixel Title */
.logo {
  font: clamp(26px, 6vw, 54px)/1.2 var(--px);
  color: var(--gold); text-align: center;
  text-shadow:
    1px 1px 0 #c2410c, 2px 2px 0 #c2410c, 3px 3px 0 #c2410c,
    4px 4px 0 #c2410c, 5px 5px 0 #c2410c, 10px 10px 0 var(--ink);
}
```

### 3. Structure the Page Layout

Organize the interface into three coordinated views:

1. **3D Perspective Selection Cards (`.cards` & `.card`)**:
   - Use a 3-column grid with `perspective: 900px`.
   - Tilt cards on hover (`.card:hover { transform: translateY(-10px) rotateX(7deg); }`) and animate pixel character avatars with a 2-step vertical bob (`animation: bob .8s steps(2) infinite`).
2. **Top HUD Bar (`.hud.px`)**:
   - Left: 12x16 pixel avatar `<canvas>`, user role title, tier tag (`.tag`), and quick-focus owner badge (`.my-badge` with `🚩`).
   - Center: Key metric pills (`.pill` with `3px solid var(--ink)` and `var(--bg2)` background).
   - Right: Live simulation status badge (`.live-pill` with blinking `.live-dot`), flow-speed toggle button, and navigation return buttons.
3. **2-Column Stage and Inspector Workspace (`.workspace`)**:
   - Use `grid-template-columns: minmax(0, 1fr) 350px; gap: 16px`.
   - **Left Stage Column (`.stage-col`)**:
     - Compact top command bar (`.cmd-bar`) for contextual actions.
     - Main canvas viewport (`.cvw`) containing the floating top-right primary tools dock (`.fin-float-dock`), bottom-left triggered narrator widget (`.narrator-box` with bouncing `.narrator-bang` exclamation mark `❗`), camera zoom rig (`.cam-rig`), and full-bleed micro-view overlay (`.micro-overlay`).
     - Bottom horizontal build and expansion dock (`.build-dock`) with icon buttons (`.pb`) and territory expansion trigger.
   - **Right Inspector Dock (`.side-dock`)**:
     - High-contrast mission prompt bar (`.mission-bar` with dark ink fill and gold border).
     - 3-tab switcher (`.dock-tabs` + `.tab-btn.on`) switching between the Station Workbench, Promotion/Scenario panel, and Voucher Ledger Log (`.ledger-item` cards with left color-coded borders).

### 4. Render 2.5D Isometric Pixel Content and Dynamic Effects

Build isometric worlds on `<canvas>` using integer pixel primitives rather than external image assets:

- **Isometric Projection**: Map grid coordinates `(x, y)` to screen coordinates via `P(x, y) = [OX + (x - y) * 10, OY + (x + y) * 5 - 3]`.
- **Isometric 3D Prism (`box`) and Diamond Tile (`dia`)**:
  - Draw a diamond top face using 10 horizontal scanlines (`h = 2 * (r < 5 ? r + 1 : 10 - r)`).
  - Draw skewed left and right vertical walls (`10px` wide each, stepping `1px` vertically every `2px` horizontally) with distinct sunlit and shaded colors to create strong 3D block depth.
- **Terrain and Water Diversity**:
  - Differentiate tile types visually: lowered river channels (`y + 2`) with animated white wave pixels, arched bridges, elevated rocky ridges (`height: 6`), fertile meadows with yellow flower pixels, and golden commercial avenues.
- **Dynamic Scene Effects**:
  - Animate rising smoke puffs on factory chimneys, sparkling white pixels (`n.fx`) on newly built or focused structures, bobbing selection arrows above active nodes, and moving cargo/coin particles along dotted connection lines.
  - Anchor crisp HTML floating badges (`#lens` and `.owner-flag`) above canvas buildings using percentage coordinates (`left: (px / W * 100)%`, `top: ((py - height) / H * 100)%`).

### 5. Implement Smooth Camera Zoom to Micro-View and Bulletproof Navigation

1. **Smooth Camera Push-In (`togglePeepView`)**:
   - When entering micro-view on a selected node, compute its percentage coordinates `(ox%, oy%)` on the canvas, set `camRig.style.transformOrigin = "${ox}% ${oy}%"`, and apply `camRig.style.transform = "scale(2.75)"` with `transition: transform .58s cubic-bezier(.22, 1, .36, 1)`.
   - Fade and scale in `.micro-overlay.open` (`opacity: 1; transform: scale(1)`).
2. **Sharp Typography in Micro-View**:
   - Render the micro-scene canvas at 2x HiDPI (`width="1040" height="520"` with `ctx.setTransform(2, 0, 0, 2, 0, 0)`).
   - Place all signboards (`.mc-dom-sign`), customer thought bubbles (`.mc-dom-speech`), and floating transaction popups (`.mc-dom-float`) in native HTML DOM elements over the canvas so text remains vector-sharp on all displays.
3. **Bulletproof Return Paths**:
   - Provide a unified `closeModal()` function bound to a top-right `✕` button, a bottom return button, the `Escape` key, and backdrop clicks, automatically restoring any auto-paused simulation speed.
   - Include a `← Back` button whenever jumping from one modal to a sub-modal, and synchronize overlay classes (`#microView.classList.toggle('open', Boolean(state.peep))`) inside the main `render()` loop.

## Gotchas

- **Blurry Canvas Text**: Global `canvas { image-rendering: pixelated; }` makes anti-aliased `fillText()` look blurry when stretched. Always use native HTML/CSS DOM plaques for signboards, labels, and speech bubbles over the canvas.
- **Trapped Modals or Overlays**: Never pause a background simulation for a random event modal without recording `pausedByModal = true` and restoring the previous speed inside a centralized `closeModal()` handler triggered by `Escape`, backdrop clicks, and close buttons.
- **Sandbox Dialog Restrictions**: Do not use native `alert()`, `confirm()`, or `prompt()`. Use in-page pixel modals (`.dlg > .px.win`), double-click confirmation states on destructive buttons (`dataset.ok`), and bottom toast notifications (`#toast`).
