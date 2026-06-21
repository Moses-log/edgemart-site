# Discord Sidebar Redesign — Design Spec

## Goal
Redesign the channel list in the "Discord Community" section of `index.html` to visually match a real Discord category sidebar (per user-supplied screenshot), and add two channels that exist in the real Discord but are missing from the site: `jerome-powell-alerts` and `hoang-trades`.

## Scope
Single file: `index.html` (HTML structure, CSS, and the existing `switchDiscordTab` JS already present — no new JS functions needed).

## 1. Sidebar component
Replace the flat `.channel-list` (currently 10 ungrouped `.ch` rows) with a grouped sidebar styled like real Discord (`#2B2D31` background, hover/active row highlight), organized into four category groups, each with an uppercase label and decorative chevron:

- **Premium Alerts** — 🚨 `moses-alerts`, 🚨 `gabe-alerts`, 🚨 `jerome-powell-alerts` *(new)*
- **Premium Bot Alerts** — 🔁 `hoang-trades` *(new)*, 🔁 `kimi-trades`, 📈 `kimi-investments`, 📈 `yabe-investments`
- **Free Channels** — 📢 `announcements`, 💬 `general`
- **In Development** — `market-research`, `earnings-analysis` (kept as disabled/dim rows, as today)

PREMIUM/FREE badges are preserved, right-aligned in each row.

## 2. New tabs + mock previews
Add two tabs to the existing `.discord-tabs` control (alongside `kimi-trades`, `moses-alerts`, `gabe-alerts`, `general`), each backed by a `.discord-mock` block toggled via the existing `switchDiscordTab()`:

- **`jerome-powell-alerts`** (🚨) — a third human trader (not a co-owner), trades swing + day setups. Avatar/name color `#FAA61A`. Two messages: one day trade, one swing trade — same alert format/style as `moses-alerts`/`gabe-alerts`.
- **`hoang-trades`** (🔁) — automated bot ("Hoang Bot", avatar color `#9B59B6`), mean-reversion / support-resistance strategy across SPY, QQQ, and auto-scanned individual stocks. Rendered as a **Discord embed card** (new component, not the plain `dm-txt` signal format used by Kimi Bot):
  - Green status dot + `Broker Mode: Active (LIVE endpoint)` header
  - Bold field labels: `Watch Long Tickers`, `Watch Short Tickers`, `Watch Long & Short Tickers`, `Time (CST)`
  - First message: exact content from user's screenshot (`MU, DELL, BSX, CSGP` / `INTC, WDC, ZTS, ADBE` / `QQQ, SPY` / `06/20/2026, 09:35 PM`)
  - Second message: earlier scan, different tickers/timestamp, same format

## 3. New CSS
- `.discord-sidebar`, `.ch-cat`, `.ch-cat-label`, `.ch-row` (+ `.active`/hover states) for the grouped sidebar
- `.d-embed`, `.d-embed-status`, `.d-embed-dot`, `.d-embed-field`, `.d-embed-field-name`, `.d-embed-field-value` for the bot embed card
- `.dm-av.jp` / `.dm-name.jp` (`#FAA61A`) for Jerome Powell trader
- `.dm-av.hoang` / `.dm-name.hoang` (`#9B59B6`) for Hoang Bot

## 4. Mobile
Per standing site rule, add `@media (max-width: 600px)` rules in the same edit for all new classes above (sidebar row padding/font-size, embed card padding/font-size) alongside the existing Discord-section mobile block (~line 925).

## Out of scope
- No backend/real Discord integration — this is a static marketing mockup, consistent with the rest of the section.
- No changes to `portfolio.html`, `stats.html`, `trades.html`.
