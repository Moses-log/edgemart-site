# Discord Sidebar Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign the flat Discord channel list in `index.html`'s Community section into a grouped, real-Discord-style sidebar, and add two missing channels (`jerome-powell-alerts`, `hoang-trades`) with matching tab previews.

**Architecture:** Single static HTML file (`index.html`) — no build step, no test runner. "Tests" in this plan are `grep` checks for unique selectors plus a manual browser check (open file directly), consistent with how the rest of this file is maintained.

**Tech Stack:** Plain HTML/CSS/vanilla JS (existing `switchDiscordTab()` function, reused unmodified).

## Global Constraints

- Single file: `C:\Users\moses\kimi-invest-site\index.html`. No other files are modified.
- Every new CSS class added in the desktop block MUST get a corresponding mobile override in the existing `@media (max-width: 600px)` block (per project's standing mobile-responsiveness rule — no desktop-only changes ship).
- Reuse existing classes where possible: `.ch-badge.premium`, `.ch-badge.free`, `.ch-wip`, `.dm-txt`, `.y` (price highlight), `.dm-time`, `.dm-hdr`. Do not redefine them.
- Channel content/order must match the four groups from the spec: Premium Alerts (moses-alerts, gabe-alerts, jerome-powell-alerts), Premium Bot Alerts (hoang-trades, kimi-trades, kimi-investments, yabe-investments), Free Channels (announcements, general), In Development (market-research, earnings-analysis).

---

### Task 1: Desktop CSS for sidebar, embed card, and new avatar colors

**Files:**
- Modify: `index.html` (CSS `<style>` block, inside the `/* ── COMMUNITY / DISCORD ── */` section, ~line 453-468, and the `/* ── DISCORD CHANNEL TABS ── */` section, ~line 578-592)

**Interfaces:**
- Produces CSS classes consumed by Task 2 (`.discord-sidebar`, `.ch-cat`, `.ch-cat-label`, `.ch-cat-add`, `.ch-row`, `.ch-row.ch-dev`) and Task 3 (`.d-embed`, `.d-embed-status`, `.d-embed-dot`, `.d-embed-field`, `.d-embed-field-name`, `.d-embed-field-value`, `.dm-av.jp`, `.dm-name.jp`, `.dm-av.hoang`, `.dm-name.hoang`).

- [ ] **Step 1: Replace the old `.channel-list`/`.ch` rules with the new sidebar rules**

Find this block (~line 459-467):

```css
    .channel-list { display: flex; flex-direction: column; gap: 6px; }
    .ch {
      display: flex; align-items: center; gap: 10px; padding: 11px 15px;
      background: var(--bg2); border: 1px solid var(--border);
      font-size: 12px; color: var(--muted); transition: border-color .2s, color .2s;
    }
    .ch:hover { border-color: var(--border-y); color: var(--white); }
    .ch-hash { color: var(--dim); font-weight: 700; }
    .ch-live { margin-left: auto; font-size: 9px; font-weight: 700; letter-spacing: 1px; color: var(--green); }
```

Replace with:

```css
    .discord-sidebar { background: #2B2D31; border: 1px solid #1E1F22; padding: 8px 6px 12px; display: flex; flex-direction: column; }
    .ch-cat { display: flex; align-items: center; justify-content: space-between; padding: 14px 8px 4px; }
    .ch-cat:first-child { padding-top: 8px; }
    .ch-cat-label { display: flex; align-items: center; gap: 5px; font-size: 11px; font-weight: 700; letter-spacing: .3px; text-transform: uppercase; color: #949BA4; }
    .ch-cat-label .chev { font-size: 9px; }
    .ch-cat-add { color: #949BA4; font-size: 15px; font-weight: 400; line-height: 1; }
    .ch-row { display: flex; align-items: center; gap: 6px; padding: 6px 8px; margin: 0 2px; border-radius: 4px; font-size: 13px; color: #949BA4; transition: background .15s, color .15s; }
    .ch-row:hover, .ch-row.active { background: #35373C; color: #F2F3F5; }
    .ch-row.active { font-weight: 600; }
    .ch-row .ch-hash { color: #80848E; font-weight: 600; font-size: 15px; }
    .ch-row .ch-badge { margin-left: auto; }
    .ch-row.ch-dev { opacity: .4; }
    .ch-row .ch-wip { margin-left: auto; font-size: 9px; font-weight: 700; letter-spacing: .5px; color: var(--accent); text-transform: uppercase; }
```

- [ ] **Step 2: Add embed-card and new avatar color CSS**

Find this line (~line 592, end of the Discord channel tabs block):

```css
    .dm-disclaimer { font-size: 10px; color: var(--dim); font-style: italic; margin-top: 6px; border-top: 1px solid #2F3136; padding-top: 6px; }
```

Replace with (keep the original line, append new rules after it):

```css
    .dm-disclaimer { font-size: 10px; color: var(--dim); font-style: italic; margin-top: 6px; border-top: 1px solid #2F3136; padding-top: 6px; }
    .dm-av.jp     { background: #FAA61A; color: #0A0800; font-size: 10px; font-weight: 800; }
    .dm-name.jp   { color: #FAA61A; }
    .dm-av.hoang  { background: #9B59B6; color: #fff; font-size: 10px; font-weight: 800; }
    .dm-name.hoang { color: #9B59B6; }
    .d-embed             { background: #2B2D31; border-left: 4px solid #23A55A; border-radius: 3px; padding: 10px 14px; margin-top: 2px; max-width: 100%; }
    .d-embed-status      { display: flex; align-items: center; gap: 7px; font-size: 13px; font-weight: 600; color: #F2F3F5; }
    .d-embed-dot         { width: 8px; height: 8px; border-radius: 50%; background: #23A55A; flex-shrink: 0; }
    .d-embed-field       { margin-top: 9px; }
    .d-embed-field-name  { font-size: 12px; font-weight: 700; color: #F2F3F5; }
    .d-embed-field-value { font-size: 12px; color: #DBDEE1; margin-top: 2px; }
```

- [ ] **Step 3: Verify the CSS was added correctly**

Run: `grep -c "discord-sidebar\|d-embed-field-value\|dm-av.hoang" "C:\Users\moses\kimi-invest-site\index.html"`
Expected: `3` (one match for each pattern, all on the CSS definition lines added above)

- [ ] **Step 4: Commit**

```bash
cd "C:\Users\moses\kimi-invest-site" && git add index.html && git commit -m "Add CSS for grouped Discord sidebar and bot embed card"
```

---

### Task 2: Replace channel-list markup with grouped sidebar HTML

**Files:**
- Modify: `index.html` (~line 1406-1417, inside `<div class="community-left fade-up">`)

**Interfaces:**
- Consumes: `.discord-sidebar`, `.ch-cat`, `.ch-cat-label`, `.ch-cat-add`, `.ch-row`, `.ch-row.ch-dev` from Task 1.
- No new interfaces produced (leaf markup).

- [ ] **Step 1: Replace the old flat channel-list div**

Find this block:

```html
        <div class="channel-list">
          <div class="ch"><span class="ch-hash">#</span><span>『🔁』kimi-trades</span><span class="ch-badge premium">PREMIUM</span></div>
          <div class="ch"><span class="ch-hash">#</span><span>『📈』kimi-investments</span><span class="ch-badge premium">PREMIUM</span></div>
          <div class="ch"><span class="ch-hash">#</span><span>『📈』yabe-investments</span><span class="ch-badge premium">PREMIUM</span></div>
          <div class="ch"><span class="ch-hash">#</span><span>『🚨』moses-alerts</span><span class="ch-badge premium">PREMIUM</span></div>
          <div class="ch"><span class="ch-hash">#</span><span>『🚨』gabe-alerts</span><span class="ch-badge premium">PREMIUM</span></div>
          <div class="ch"><span class="ch-hash">#</span><span>『💬』premium-general</span><span class="ch-badge premium">PREMIUM</span></div>
          <div class="ch"><span class="ch-hash">#</span><span>『📢』announcements</span><span class="ch-badge free">FREE</span></div>
          <div class="ch"><span class="ch-hash">#</span><span>『💬』general</span><span class="ch-badge free">FREE</span></div>
          <div class="ch ch-dev"><span class="ch-hash">#</span><span>market-research</span><span class="ch-wip">◆ IN DEVELOPMENT</span></div>
          <div class="ch ch-dev"><span class="ch-hash">#</span><span>earnings-analysis</span><span class="ch-wip">◆ IN DEVELOPMENT</span></div>
        </div>
```

Replace with:

```html
        <div class="discord-sidebar">
          <div class="ch-cat">
            <span class="ch-cat-label"><span class="chev">▾</span> Premium Alerts</span>
            <span class="ch-cat-add">+</span>
          </div>
          <div class="ch-row"><span class="ch-hash">#</span><span>『🚨』moses-alerts</span><span class="ch-badge premium">PREMIUM</span></div>
          <div class="ch-row"><span class="ch-hash">#</span><span>『🚨』gabe-alerts</span><span class="ch-badge premium">PREMIUM</span></div>
          <div class="ch-row"><span class="ch-hash">#</span><span>『🚨』jerome-powell-alerts</span><span class="ch-badge premium">PREMIUM</span></div>

          <div class="ch-cat">
            <span class="ch-cat-label"><span class="chev">▾</span> Premium Bot Alerts</span>
            <span class="ch-cat-add">+</span>
          </div>
          <div class="ch-row"><span class="ch-hash">#</span><span>『🔁』hoang-trades</span><span class="ch-badge premium">PREMIUM</span></div>
          <div class="ch-row"><span class="ch-hash">#</span><span>『🔁』kimi-trades</span><span class="ch-badge premium">PREMIUM</span></div>
          <div class="ch-row"><span class="ch-hash">#</span><span>『📈』kimi-investments</span><span class="ch-badge premium">PREMIUM</span></div>
          <div class="ch-row"><span class="ch-hash">#</span><span>『📈』yabe-investments</span><span class="ch-badge premium">PREMIUM</span></div>

          <div class="ch-cat">
            <span class="ch-cat-label"><span class="chev">▾</span> Free Channels</span>
          </div>
          <div class="ch-row"><span class="ch-hash">#</span><span>『📢』announcements</span><span class="ch-badge free">FREE</span></div>
          <div class="ch-row"><span class="ch-hash">#</span><span>『💬』general</span><span class="ch-badge free">FREE</span></div>

          <div class="ch-cat">
            <span class="ch-cat-label"><span class="chev">▾</span> In Development</span>
          </div>
          <div class="ch-row ch-dev"><span class="ch-hash">#</span><span>market-research</span><span class="ch-wip">◆ IN DEVELOPMENT</span></div>
          <div class="ch-row ch-dev"><span class="ch-hash">#</span><span>earnings-analysis</span><span class="ch-wip">◆ IN DEVELOPMENT</span></div>
        </div>
```

(Note: `premium-general` channel row is intentionally dropped from the sidebar list — it wasn't in the real Discord screenshot's categories; the tab/preview for it on the right is untouched.)

- [ ] **Step 2: Verify the new markup is in place**

Run: `grep -c "jerome-powell-alerts\|hoang-trades\|ch-cat-label" "C:\Users\moses\kimi-invest-site\index.html"`
Expected: count > 0 for each (jerome-powell-alerts and hoang-trades will each match twice once Task 3 lands; right now expect at least 1 match each from the sidebar row, plus 4 `ch-cat-label` matches)

- [ ] **Step 3: Commit**

```bash
cd "C:\Users\moses\kimi-invest-site" && git add index.html && git commit -m "Replace flat channel list with grouped Discord sidebar"
```

---

### Task 3: Add hoang-trades and jerome-powell-alerts tabs + mock previews

**Files:**
- Modify: `index.html` (~line 1421-1426 for tabs, ~line 1429-1527 for mock blocks)

**Interfaces:**
- Consumes: `.d-embed*` and `.dm-av.jp`/`.dm-av.hoang` classes from Task 1; existing `switchDiscordTab()` JS (no signature change — called as `switchDiscordTab('<channel-id>', this)`).
- Produces: two new `#dt-hoang-trades` and `#dt-jerome-powell-alerts` DOM ids that `switchDiscordTab()` toggles by string match (`'dt-' + tab`), so the tab `onclick` argument MUST exactly equal the id suffix.

- [ ] **Step 1: Add the two new tab buttons**

Find this block (~line 1421-1426):

```html
        <div class="discord-tabs">
          <button class="d-tab active" onclick="switchDiscordTab('kimi-trades',this)">🔁 kimi-trades</button>
          <button class="d-tab" onclick="switchDiscordTab('moses-alerts',this)">🚨 moses-alerts</button>
          <button class="d-tab" onclick="switchDiscordTab('gabe-alerts',this)">🚨 gabe-alerts</button>
          <button class="d-tab" onclick="switchDiscordTab('premium-general',this)">💬 general</button>
        </div>
```

Replace with:

```html
        <div class="discord-tabs">
          <button class="d-tab active" onclick="switchDiscordTab('kimi-trades',this)">🔁 kimi-trades</button>
          <button class="d-tab" onclick="switchDiscordTab('hoang-trades',this)">🔁 hoang-trades</button>
          <button class="d-tab" onclick="switchDiscordTab('moses-alerts',this)">🚨 moses-alerts</button>
          <button class="d-tab" onclick="switchDiscordTab('gabe-alerts',this)">🚨 gabe-alerts</button>
          <button class="d-tab" onclick="switchDiscordTab('jerome-powell-alerts',this)">🚨 jerome-powell</button>
          <button class="d-tab" onclick="switchDiscordTab('premium-general',this)">💬 general</button>
        </div>
```

- [ ] **Step 2: Insert the hoang-trades mock block right after the kimi-trades mock block**

Find the closing of the kimi-trades mock (~line 1459, the end of `<div class="discord-mock" id="dt-kimi-trades">`):

```html
            </div>
          </div>
        </div>

        <!-- moses-alerts mock -->
```

Replace with:

```html
            </div>
          </div>
        </div>

        <!-- hoang-trades mock -->
        <div class="discord-mock" id="dt-hoang-trades" style="display:none">
          <div class="discord-top">
            <span class="d-hash">#</span>
            <span class="d-ch-name">『🔁』hoang-trades</span>
          </div>
          <div class="discord-msgs">
            <div class="dm">
              <div class="dm-av hoang">HB</div>
              <div>
                <div class="dm-hdr"><span class="dm-name hoang">Hoang Bot</span><span class="dm-time">Today at 2:10 PM</span></div>
                <div class="d-embed">
                  <div class="d-embed-status"><span class="d-embed-dot"></span>Broker Mode: Active (LIVE endpoint)</div>
                  <div class="d-embed-field">
                    <div class="d-embed-field-name">Watch Long Tickers</div>
                    <div class="d-embed-field-value">AVGO, LRCX, ON, STX</div>
                  </div>
                  <div class="d-embed-field">
                    <div class="d-embed-field-name">Watch Short Tickers</div>
                    <div class="d-embed-field-value">PARA, LUMN, RIVN, SNAP</div>
                  </div>
                  <div class="d-embed-field">
                    <div class="d-embed-field-name">Watch Long &amp; Short Tickers</div>
                    <div class="d-embed-field-value">SPY, QQQ</div>
                  </div>
                  <div class="d-embed-field">
                    <div class="d-embed-field-name">Time (CST)</div>
                    <div class="d-embed-field-value">06/20/2026, 02:10 PM</div>
                  </div>
                </div>
              </div>
            </div>
            <div class="dm">
              <div class="dm-av hoang">HB</div>
              <div>
                <div class="dm-hdr"><span class="dm-name hoang">Hoang Bot</span><span class="dm-time">Today at 9:35 PM</span></div>
                <div class="d-embed">
                  <div class="d-embed-status"><span class="d-embed-dot"></span>Broker Mode: Active (LIVE endpoint)</div>
                  <div class="d-embed-field">
                    <div class="d-embed-field-name">Watch Long Tickers</div>
                    <div class="d-embed-field-value">MU, DELL, BSX, CSGP</div>
                  </div>
                  <div class="d-embed-field">
                    <div class="d-embed-field-name">Watch Short Tickers</div>
                    <div class="d-embed-field-value">INTC, WDC, ZTS, ADBE</div>
                  </div>
                  <div class="d-embed-field">
                    <div class="d-embed-field-name">Watch Long &amp; Short Tickers</div>
                    <div class="d-embed-field-value">QQQ, SPY</div>
                  </div>
                  <div class="d-embed-field">
                    <div class="d-embed-field-name">Time (CST)</div>
                    <div class="d-embed-field-value">06/20/2026, 09:35 PM</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- moses-alerts mock -->
```

- [ ] **Step 3: Insert the jerome-powell-alerts mock block right after the gabe-alerts mock block**

Find the closing of the gabe-alerts mock (~line 1526, immediately before the premium-general comment):

```html
          </div>
        </div>
        <!-- premium-general mock -->
```

Replace with:

```html
          </div>
        </div>

        <!-- jerome-powell-alerts mock -->
        <div class="discord-mock" id="dt-jerome-powell-alerts" style="display:none">
          <div class="discord-top">
            <span class="d-hash">#</span>
            <span class="d-ch-name">『🚨』jerome-powell-alerts</span>
          </div>
          <div class="discord-msgs">
            <div class="dm">
              <div class="dm-av jp">JP</div>
              <div>
                <div class="dm-hdr"><span class="dm-name jp">Jerome Powell</span><span class="dm-time">Today at 9:48 AM</span></div>
                <div class="dm-txt">
                  🚨 <strong style="color:#fff">DAY TRADE — SOFI (long)</strong><br>
                  @ <span class="y">$14.82</span><br>
                  Type: Day trade | Scalping the open<br>
                  Thesis: Gap fill setup off the open, riding momentum into first resistance at $15.20. Out by close either way.
                </div>
              </div>
            </div>
            <div class="dm">
              <div class="dm-av jp">JP</div>
              <div>
                <div class="dm-hdr"><span class="dm-name jp">Jerome Powell</span><span class="dm-time">Today at 1:33 PM</span></div>
                <div class="dm-txt">
                  📋 <strong style="color:#fff">SWING — PLTR (short-term)</strong><br>
                  @ <span class="y">$142.65</span><br>
                  Type: Swing trade | 3–5 day hold<br>
                  Holding through the next leg up — stop under $137. Will trim into strength if it runs.
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- premium-general mock -->
```

- [ ] **Step 4: Verify both new mock blocks and tabs exist with matching ids**

Run: `grep -n "dt-hoang-trades\|dt-jerome-powell-alerts" "C:\Users\moses\kimi-invest-site\index.html"`
Expected: 2 lines for each id — one `<button onclick="switchDiscordTab('hoang-trades',this)">`-style reference matches `'hoang-trades'` (not literally `dt-hoang-trades`, so re-run without `dt-` prefix if needed) and one `id="dt-hoang-trades"` div; same for jerome-powell-alerts. Confirm no typos (`switchDiscordTab` builds the id as `'dt-' + tab`, so the button's first argument string must exactly equal the part after `dt-` in the div id).

- [ ] **Step 5: Commit**

```bash
cd "C:\Users\moses\kimi-invest-site" && git add index.html && git commit -m "Add hoang-trades and jerome-powell-alerts tabs and mock previews"
```

---

### Task 4: Mobile CSS for all new classes

**Files:**
- Modify: `index.html` (existing `@media (max-width: 600px)` block, `/* ── Discord community section ── */` subsection, ~line 925-939)

**Interfaces:**
- Consumes: all classes from Task 1 (`.discord-sidebar`, `.ch-cat`, `.ch-cat-label`, `.ch-row`, `.d-embed*`).

- [ ] **Step 1: Add mobile overrides**

Find this block (~line 925-939):

```css
      /* ── Discord community section ── */
      .community-left h3   { font-size: 22px; letter-spacing: -0.5px; }
      .community-left p    { font-size: 13px; }
      .ch                  { padding: 10px 12px; font-size: 11px; }

      /* Discord tabs: scroll horizontally instead of wrapping/overflowing */
      .discord-tabs        { overflow-x: auto; -webkit-overflow-scrolling: touch;
                             flex-wrap: nowrap; padding-bottom: 4px; gap: 4px; }
      .d-tab               { flex-shrink: 0; padding: 6px 11px; font-size: 10px; letter-spacing: 1px; }

      /* Discord mock messages */
      .discord-msgs        { padding: 10px 12px; gap: 10px; }
      .dm-txt              { font-size: 11px; }
      .dm-name             { font-size: 11px; }
      .dm-time             { font-size: 9px; }
```

Replace with:

```css
      /* ── Discord community section ── */
      .community-left h3   { font-size: 22px; letter-spacing: -0.5px; }
      .community-left p    { font-size: 13px; }

      /* Discord sidebar (grouped channel list) */
      .discord-sidebar     { padding: 6px 4px 10px; }
      .ch-cat               { padding: 10px 6px 4px; }
      .ch-cat-label         { font-size: 10px; }
      .ch-row               { padding: 7px 6px; font-size: 12px; }
      .ch-row .ch-hash      { font-size: 13px; }

      /* Discord tabs: scroll horizontally instead of wrapping/overflowing */
      .discord-tabs        { overflow-x: auto; -webkit-overflow-scrolling: touch;
                             flex-wrap: nowrap; padding-bottom: 4px; gap: 4px; }
      .d-tab               { flex-shrink: 0; padding: 6px 11px; font-size: 10px; letter-spacing: 1px; }

      /* Discord mock messages */
      .discord-msgs        { padding: 10px 12px; gap: 10px; }
      .dm-txt              { font-size: 11px; }
      .dm-name             { font-size: 11px; }
      .dm-time             { font-size: 9px; }

      /* Bot embed card */
      .d-embed              { padding: 8px 10px; }
      .d-embed-status       { font-size: 12px; }
      .d-embed-field-name   { font-size: 11px; }
      .d-embed-field-value  { font-size: 11px; }
```

(Note: the old bare `.ch { padding: 10px 12px; font-size: 11px; }` rule is removed since `.ch` no longer exists in the markup after Task 2 — it's replaced by `.ch-row`.)

- [ ] **Step 2: Verify no orphaned `.ch` selector remains and new mobile rules exist**

Run: `grep -n "^\s*\.ch \|d-embed-field-value.*font-size: 11px" "C:\Users\moses\kimi-invest-site\index.html"`
Expected: no match for the bare `.ch ` selector; one match for the mobile `.d-embed-field-value` rule.

- [ ] **Step 3: Commit**

```bash
cd "C:\Users\moses\kimi-invest-site" && git add index.html && git commit -m "Add mobile CSS for grouped Discord sidebar and bot embed card"
```

---

### Task 5: Manual browser verification

**Files:** None (verification only).

- [ ] **Step 1: Open the site locally**

Run: `start "C:\Users\moses\kimi-invest-site\index.html"` (or open directly in a browser)

- [ ] **Step 2: Desktop check**

Scroll to the "Discord Community" section. Confirm:
- Sidebar shows 4 category headers (Premium Alerts, Premium Bot Alerts, Free Channels, In Development) with channel rows grouped correctly and PREMIUM/FREE badges visible.
- Clicking each of the 6 tabs (kimi-trades, hoang-trades, moses-alerts, gabe-alerts, jerome-powell, general) swaps the preview panel with no layout break.
- The `hoang-trades` tab shows two embed cards with the green dot, bold field labels, and ticker values rendering correctly (no overflow, `&amp;` renders as `&`).

- [ ] **Step 3: Mobile check**

Resize the browser to ~390px width (or use device toolbar). Confirm:
- Sidebar rows and category labels shrink and remain readable, no horizontal overflow.
- Discord tabs scroll horizontally without wrapping.
- Embed card text is legible and not clipped.

- [ ] **Step 4: Report back**

No commit for this task — if any visual issue is found, fix it as a follow-up edit to the relevant task's CSS/HTML and commit separately.
