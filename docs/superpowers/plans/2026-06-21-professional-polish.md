# Professional Polish Pass Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace 16 colorful emoji-as-icons with a consistent SVG line-icon set, remove 3 inline decorative emoji from copy, and tone down 4 ambient/looping animations — without changing layout, copy, color palette, or font.

**Architecture:** Single static HTML file (`index.html`), no build step. Icons are inline SVG (`viewBox="0 0 24 24"`, `fill="none"`, `stroke="currentColor"`, `stroke-width="2"`, `stroke-linecap="round"`, `stroke-linejoin="round"`) so they inherit color from CSS, matching the existing inline-style-driven color conventions already used throughout this file.

**Tech Stack:** Plain HTML/CSS/inline SVG. No new dependencies, no JS changes.

## Global Constraints

- Single file: `C:\Users\moses\kimi-invest-site\index.html`. No other files are modified.
- Do not touch: the Discord Community preview section's emoji (lines ~1437-1700, channel icons and chat messages), the `✓`/`✕`/`✦` glyphs anywhere, any layout/copy/color-palette/font, any purposeful hover/click/transition animation (button hover, FAQ accordion, Discord tab switching via `switchDiscordTab()`).
- No mobile CSS changes needed for the icon swap (verified: no existing `@media (max-width: 600px)` overrides reference `.benefit-icon`, `.ss-icon`, or `.sig-icon`, and container dimensions are not changing).
- Every new SVG must use exactly this attribute set so they render consistently: `fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"`.

---

### Task 1: Benefit-card icons (9 instances)

**Files:**
- Modify: `index.html` lines 343-347 (CSS) and lines 1326, 1332, 1338, 1344, 1350, 1356, 1362, 1368, 1374 (HTML)

**Interfaces:**
- Produces: `.benefit-icon` gets `color: var(--accent)` so all 9 SVGs (via `stroke="currentColor"`) render in the brand yellow — no per-icon color needed.

- [ ] **Step 1: Add color to the shared `.benefit-icon` CSS rule**

Find:
```css
    .benefit-icon {
      width: 46px; height: 46px; background: var(--adim); border: 1px solid var(--border-y);
      display: flex; align-items: center; justify-content: center;
      font-size: 18px; margin-bottom: 24px;
    }
```

Replace with:
```css
    .benefit-icon {
      width: 46px; height: 46px; background: var(--adim); border: 1px solid var(--border-y);
      display: flex; align-items: center; justify-content: center;
      color: var(--accent); margin-bottom: 24px;
    }
```

- [ ] **Step 2: Replace each of the 9 emoji with an SVG icon**

Find: `<div class="benefit-icon">📡</div>`
Replace:
```html
<div class="benefit-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="22" height="22"><circle cx="12" cy="19" r="1.4" fill="currentColor" stroke="none"/><path d="M7 16a7 7 0 0 1 10 0"/><path d="M3.5 12.5a11 11 0 0 1 17 0"/></svg></div>
```

Find: `<div class="benefit-icon">🤖</div>`
Replace:
```html
<div class="benefit-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="22" height="22"><rect x="4" y="8" width="16" height="12" rx="2"/><line x1="12" y1="8" x2="12" y2="4"/><circle cx="12" cy="4" r="1" fill="currentColor" stroke="none"/><line x1="9" y1="13" x2="9" y2="15"/><line x1="15" y1="13" x2="15" y2="15"/></svg></div>
```

Find: `<div class="benefit-icon">📊</div>`
Replace:
```html
<div class="benefit-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="22" height="22"><line x1="5" y1="20" x2="5" y2="14"/><line x1="12" y1="20" x2="12" y2="8"/><line x1="19" y1="20" x2="19" y2="11"/></svg></div>
```

Find: `<div class="benefit-icon">🎓</div>`
Replace:
```html
<div class="benefit-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="22" height="22"><path d="M12 4 2 9l10 5 10-5-10-5z"/><path d="M6 11.5V17c0 1 2.5 2 6 2s6-1 6-2v-5.5"/></svg></div>
```

Find: `<div class="benefit-icon">👥</div>`
Replace:
```html
<div class="benefit-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="22" height="22"><circle cx="9" cy="8" r="3"/><path d="M3 20c0-3.3 2.7-6 6-6s6 2.7 6 6"/><circle cx="17" cy="8.5" r="2.5"/><path d="M15.5 14.2c2.6.5 4.5 2.8 4.5 5.8"/></svg></div>
```

Find: `<div class="benefit-icon">🔒</div>`
Replace:
```html
<div class="benefit-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="22" height="22"><rect x="5" y="11" width="14" height="9" rx="2"/><path d="M8 11V7a4 4 0 0 1 8 0v4"/></svg></div>
```

Find: `<div class="benefit-icon">🚨</div>`
Replace:
```html
<div class="benefit-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="22" height="22"><path d="M6 16V11a6 6 0 1 1 12 0v5"/><path d="M4 16h16"/><path d="M10 19a2 2 0 0 0 4 0"/></svg></div>
```

Find: `<div class="benefit-icon">🏛️</div>`
Replace:
```html
<div class="benefit-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="22" height="22"><path d="M3 9 12 4l9 5"/><line x1="5" y1="9" x2="5" y2="19"/><line x1="9" y1="9" x2="9" y2="19"/><line x1="15" y1="9" x2="15" y2="19"/><line x1="19" y1="9" x2="19" y2="19"/><line x1="3" y1="19" x2="21" y2="19"/></svg></div>
```

Find: `<div class="benefit-icon">🧑‍💼</div>`
Replace:
```html
<div class="benefit-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="22" height="22"><circle cx="12" cy="8" r="3.5"/><path d="M5 20c0-3.9 3.1-7 7-7s7 3.1 7 7"/></svg></div>
```

- [ ] **Step 3: Verify all 9 replaced and none remain**

Run: `grep -c 'benefit-icon">' index.html` (cwd `C:\Users\moses\kimi-invest-site`)
Expected: `9` (each now contains an `<svg>` — confirm none of the 9 lines still contain a raw emoji character by visually scanning the diff)

- [ ] **Step 4: Commit**

```bash
git add index.html && git commit -m "Replace benefit-card emoji icons with SVG line icons"
```

---

### Task 2: Signal-stream icons (4 instances)

**Files:**
- Modify: `index.html` lines 1393, 1400, 1407, 1414

**Interfaces:**
- Consumes: existing `.ss-who.ai` (`#5865F2`), `.ss-who.moses` (`#ED4245`), `.ss-who.yabe` (`#57F287`) identity colors (defined at lines 573-576) — reused here as inline colors on the icon wrapper, not modified.
- No new CSS rules; each icon gets its color via an inline `style` attribute on the wrapper div, matching this file's existing convention of one-off inline colors.

- [ ] **Step 1: Replace each of the 4 emoji with a colored SVG icon**

Find: `<div class="ss-icon">🤖</div>` (first occurrence, the AI Trading Signals card)
Replace:
```html
<div class="ss-icon" style="color:#5865F2"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="26" height="26"><rect x="4" y="8" width="16" height="12" rx="2"/><line x1="12" y1="8" x2="12" y2="4"/><circle cx="12" cy="4" r="1" fill="currentColor" stroke="none"/><line x1="9" y1="13" x2="9" y2="15"/><line x1="15" y1="13" x2="15" y2="15"/></svg></div>
```

Find: `<div class="ss-icon">🚨</div>` (the Co-owner Personal Trades card)
Replace:
```html
<div class="ss-icon" style="color:#ED4245"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="26" height="26"><path d="M6 16V11a6 6 0 1 1 12 0v5"/><path d="M4 16h16"/><path d="M10 19a2 2 0 0 0 4 0"/></svg></div>
```

Find: `<div class="ss-icon">🏛️</div>` (the Politician Insider Trades card)
Replace:
```html
<div class="ss-icon" style="color:#57F287"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="26" height="26"><path d="M3 9 12 4l9 5"/><line x1="5" y1="9" x2="5" y2="19"/><line x1="9" y1="9" x2="9" y2="19"/><line x1="15" y1="9" x2="15" y2="19"/><line x1="19" y1="9" x2="19" y2="19"/><line x1="3" y1="19" x2="21" y2="19"/></svg></div>
```

Find: `<div class="ss-icon">🧑‍💼</div>` (the dimmed Vetted Investor Alerts dev card)
Replace:
```html
<div class="ss-icon" style="color:var(--dim)"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="26" height="26"><circle cx="12" cy="8" r="3.5"/><path d="M5 20c0-3.9 3.1-7 7-7s7 3.1 7 7"/></svg></div>
```

- [ ] **Step 2: Verify**

Run: `grep -c 'ss-icon"' index.html` (cwd `C:\Users\moses\kimi-invest-site`)
Expected: `4`, each now containing `<svg>` and an inline `style="color:..."`.

- [ ] **Step 3: Commit**

```bash
git add index.html && git commit -m "Replace signal-stream emoji icons with colored SVG line icons"
```

---

### Task 3: Track-record signal icons (4 instances) + color CSS

**Files:**
- Modify: `index.html` lines 393-397 (CSS) and lines 1250, 1255, 1260, 1265 (HTML)

**Interfaces:**
- Consumes: existing `.signal-card.buy/.dd/.trim/.sell` modifier classes (lines 393-396, already present, not modified) whose `border-bottom` colors establish the per-signal-type identity color this task reuses.
- Produces: 4 new CSS rules scoping `.sig-icon` color to its parent `.signal-card` type.

- [ ] **Step 1: Add 4 color rules right after the existing `.sig-icon` rule**

Find:
```css
    .sig-icon { font-size: 20px; margin-bottom: 10px; }
```

Replace with:
```css
    .sig-icon { margin-bottom: 10px; }
    .signal-card.buy .sig-icon  { color: var(--green); }
    .signal-card.dd .sig-icon   { color: var(--accent); }
    .signal-card.trim .sig-icon { color: #FFA032; }
    .signal-card.sell .sig-icon { color: var(--red); }
```

- [ ] **Step 2: Replace each of the 4 emoji with an SVG icon**

Find: `<div class="sig-icon">🟢</div>`
Replace:
```html
<div class="sig-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="20" height="20"><circle cx="12" cy="12" r="9"/><polyline points="8 12.5 11 15.5 16 9"/></svg></div>
```

Find: `<div class="sig-icon">🔥</div>`
Replace:
```html
<div class="sig-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="20" height="20"><path d="M12 21c3.3 0 6-2.5 6-6 0-2.8-1.6-4.3-3-6.5.3 1.8-.5 3-1.7 2.6.7-2.6-.6-5.4-3-7.1.2 2.4-1 4-2.5 5.6C6.4 11.1 6 13 6 15c0 3.5 2.7 6 6 6z"/></svg></div>
```

Find: `<div class="sig-icon">✂️</div>`
Replace:
```html
<div class="sig-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="20" height="20"><circle cx="6" cy="6" r="2.5"/><circle cx="6" cy="18" r="2.5"/><line x1="8" y1="7.5" x2="20" y2="17"/><line x1="8" y1="16.5" x2="20" y2="7"/></svg></div>
```

Find: `<div class="sig-icon">🔴</div>`
Replace:
```html
<div class="sig-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="20" height="20"><circle cx="12" cy="12" r="9"/><line x1="9" y1="9" x2="15" y2="15"/><line x1="15" y1="9" x2="9" y2="15"/></svg></div>
```

- [ ] **Step 3: Verify**

Run: `grep -c 'sig-icon">' index.html` (cwd `C:\Users\moses\kimi-invest-site`)
Expected: `4`, each containing `<svg>`.

- [ ] **Step 4: Commit**

```bash
git add index.html && git commit -m "Replace track-record signal emoji icons with SVG line icons"
```

---

### Task 4: Remove inline decorative emoji from copy (3 locations)

**Files:**
- Modify: `index.html` lines 1034, 1851, 1927

**Interfaces:** None (leaf text edits, no shared state).

- [ ] **Step 1: Remove 🔒 from the early-access banner**

Find:
```html
  <strong>🔒 Early Access Pricing</strong> &nbsp;·&nbsp; $14.99/mo is the lowest this will ever be. Price increases when Early Access closes — lock in now.
```

Replace with:
```html
  <strong>Early Access Pricing</strong> &nbsp;·&nbsp; $14.99/mo is the lowest this will ever be. Price increases when Early Access closes — lock in now.
```

- [ ] **Step 2: Remove 🔒 from the pricing card note**

Find:
```html
      <div style="margin-top:10px;font-size:11px;color:var(--accent);font-weight:700;letter-spacing:1px;">🔒 Early Access rate — the lowest this will ever be. Price goes up when spots fill.</div>
```

Replace with:
```html
      <div style="margin-top:10px;font-size:11px;color:var(--accent);font-weight:700;letter-spacing:1px;">Early Access rate — the lowest this will ever be. Price goes up when spots fill.</div>
```

- [ ] **Step 3: Remove ⚠️ from the scammer warning title**

Find:
```html
      <div class="scammer-warn-title">⚠️ Scammer Warning</div>
```

Replace with:
```html
      <div class="scammer-warn-title">Scammer Warning</div>
```

- [ ] **Step 4: Verify none of the 3 emoji remain at these locations**

Run: `grep -n 'Early Access Pricing\|Early Access rate\|Scammer Warning' index.html` (cwd `C:\Users\moses\kimi-invest-site`)
Expected: 3 lines printed, none containing 🔒 or ⚠️.

- [ ] **Step 5: Commit**

```bash
git add index.html && git commit -m "Remove decorative emoji from banner and warning copy"
```

---

### Task 5: Tone down ambient/looping animations

**Files:**
- Modify: `index.html` lines 60 (spotlight gradient), 97/170/544/553 (btn-glow usages), 173 (btn-glow keyframe), 203-206 (fc-float usages), 207 (fc-float keyframe), 149/784 (pulse-dot usages), 151 (pulse-dot keyframe)

**Interfaces:** None (CSS-only, no shared state beyond the keyframe names already in use, which are not renamed).

- [ ] **Step 1: Reduce the spotlight gradient radius and opacity**

Find:
```css
      background: radial-gradient(700px circle at var(--mx,50%) var(--my,50%), rgba(245,230,66,0.025), transparent 70%);
```

Replace with:
```css
      background: radial-gradient(500px circle at var(--mx,50%) var(--my,50%), rgba(245,230,66,0.015), transparent 70%);
```

- [ ] **Step 2: Reduce the btn-glow keyframe intensity**

Find:
```css
    @keyframes btn-glow { 0%,100%{box-shadow:0 0 0 transparent;} 50%{box-shadow:0 0 28px rgba(245,230,66,.32);} }
```

Replace with:
```css
    @keyframes btn-glow { 0%,100%{box-shadow:0 0 0 transparent;} 50%{box-shadow:0 0 17px rgba(245,230,66,.19);} }
```

- [ ] **Step 3: Slow down all 4 btn-glow usages from 3s to 4.5s**

Find each of these 4 occurrences (they are identical strings appearing at 4 separate locations — lines 97, 170, 544, 553 — replace all 4):
```css
animation: btn-glow 3s ease-in-out infinite;
```

Replace each with:
```css
animation: btn-glow 4.5s ease-in-out infinite;
```

(Use a global find/replace across the file for this exact string — there are exactly 4 occurrences, all of which should change identically. Confirm count before and after.)

- [ ] **Step 4: Reduce the fc-float keyframe amplitude**

Find:
```css
    @keyframes fc-float { 0%,100%{transform:translateY(0);} 50%{transform:translateY(-10px);} }
```

Replace with:
```css
    @keyframes fc-float { 0%,100%{transform:translateY(0);} 50%{transform:translateY(-5px);} }
```

(Durations and delays on the 4 `.float-card:nth-child(N)` rules at lines 203-206 stay unchanged — only the keyframe amplitude changes.)

- [ ] **Step 5: Reduce the pulse-dot keyframe intensity**

Find:
```css
    @keyframes pulse-dot { 0%,100% { opacity:1; transform:scale(1); } 50% { opacity:.4; transform:scale(.7); } }
```

Replace with:
```css
    @keyframes pulse-dot { 0%,100% { opacity:1; transform:scale(1); } 50% { opacity:.7; transform:scale(.85); } }
```

- [ ] **Step 6: Slow down both pulse-dot usages from 2s to 2.5s**

Find (line 149):
```css
      border-radius: 50%; animation: pulse-dot 2s ease-in-out infinite;
```

Replace with:
```css
      border-radius: 50%; animation: pulse-dot 2.5s ease-in-out infinite;
```

Find (line 784):
```css
    .edu-coming-badge .dot { width: 8px; height: 8px; background: var(--accent); border-radius: 50%; animation: pulse-dot 2s ease-in-out infinite; }
```

Replace with:
```css
    .edu-coming-badge .dot { width: 8px; height: 8px; background: var(--accent); border-radius: 50%; animation: pulse-dot 2.5s ease-in-out infinite; }
```

- [ ] **Step 7: Verify no 3s btn-glow or 2s pulse-dot usages remain**

Run: `grep -c "btn-glow 3s\|pulse-dot 2s ease" index.html` (cwd `C:\Users\moses\kimi-invest-site`)
Expected: `0`

Run: `grep -c "btn-glow 4.5s" index.html`
Expected: `4`

- [ ] **Step 8: Commit**

```bash
git add index.html && git commit -m "Tone down ambient glow, float, pulse, and spotlight animations"
```

---

### Task 6: Manual verification

**Files:** None (verification only).

- [ ] **Step 1: Open the site locally**

Run: `start "C:\Users\moses\kimi-invest-site\index.html"` (or open directly in a browser)

- [ ] **Step 2: Visual check of every replaced icon**

Confirm each of the following renders as a clean, recognizable line icon (not a broken/empty box, not a raw emoji):
- Benefits section (9 cards): broadcast, bot, bar-chart, graduation cap, two-person, padlock, bell, building, single-person — all in yellow (`var(--accent)`)
- "Who's Alerting You" section (4 cards): bot icon in blue, bell icon in red, building icon in green, single-person icon in dim gray
- Track Record signal legend (4 items): green check-circle, yellow flame, orange scissors, red x-circle

If any icon looks visually broken (e.g. a stray line, disproportionate shape), adjust that icon's path data slightly and re-save — minor curve/shape tuning is expected polish, not a spec violation, since the spec only mandates the icon's meaning and color, not exact path coordinates.

- [ ] **Step 3: Confirm removed emoji are gone**

Confirm the early-access banner, the pricing card note, and the "Scammer Warning" heading no longer show 🔒 or ⚠️, and read cleanly without them.

- [ ] **Step 4: Confirm animations are visibly subtler, not absent**

Confirm the trial badge, free-tag, and large CTA button still pulse a glow (just smaller/slower), the 4 floating hero cards still bob gently (smaller amplitude), the hero badge dot still pulses (smaller/slower), and the mouse-follow spotlight is still present but subtler.

- [ ] **Step 5: Confirm nothing out of scope changed**

Confirm the Discord Community preview section still shows its original emoji unchanged, layout/copy/colors/fonts are unchanged, and all hover/click/transition interactions (buttons, FAQ accordion, Discord tabs) still work exactly as before.

No commit for this task — if any issue is found, fix it as a follow-up edit to the relevant task and commit separately.
