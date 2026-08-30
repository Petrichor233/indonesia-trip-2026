---
name: trip-assistant
description: Generate or update a couple/family mobile trip-assistant web app that matches the Indonesia trip-assistant framework (今日/行程/地图/准备, TextDB sync, spend ledger, packing). Use whenever the user asks to 做行程助手, 旅行助手, 出行助手, 行程网站, trip assistant, itinerary app, travel helper, or to clone/adapt the Indonesia trip helper for another destination or dates.
---

# Trip Assistant Generator

Build a **single-file** trip site whose chrome, tabs, sync, and catalogs match the Indonesia trip assistant. Do not invent a different IA.

Read these before writing code:

- [references/intake.md](references/intake.md) — what to ask, what may stay empty
- [references/blueprint.md](references/blueprint.md) — pages, data shapes, sync, map, spend
- [references/preferences.md](references/preferences.md) — taste learned from the Indonesia build

If this repo already has that assistant (`index.html` with `page-today` / `cloud.lists` / TextDB), **copy its HTML/CSS/JS chrome** and replace only trip-specific catalogs and copy. Do not paste Indonesia days, flights, or hotels into a new trip.

## When this skill applies

Use it for new trips **and** later edits that stay inside this product (change a hotel, restack spend bars, add a map leg, tweak sync). If the user only wants a static PDF or a Notes list, do not use this skill.

## Interactive first — do not skip

This skill is interactive. If the brief is empty or thin, **ask before generating catalogs**. Do not invent flight numbers, hotel names, prices, or a day-by-day plot.

1. Ask the **must-have** block in [intake.md](references/intake.md).
2. Ask the **optional** block in one short list. Missing items stay empty (`[]`, `待预订`, `待补`, `status:"wait"`).
3. If the user says they do not have it yet, leave it empty and continue. Do not block the whole app on optional data.
4. Confirm destination, date range, and who is traveling, then scaffold.
5. When they later send screenshots or order numbers, patch catalogs in place. Never reset `cloud.lists`.

Ask in the user's language (default 中文). Batch questions; do not send twenty one-liners.

## Output shape (must stay consistent)

| Layer | Rule |
|---|---|
| Files | `index.html` + optional `img/acts/*.webp`. No SPA framework. |
| Nav | Bottom: 今日 / 行程 / 地图 / 准备 |
| 行程 | 日程 / 航班 / 酒店 / 活动 |
| 准备 | 待办 / 行李 / 花费 |
| Language | Chinese UI; compact cards; Feishu-like polish without copying Feishu |
| Sync | TextDB couple sync, PIN unlock, local draft; see blueprint |
| Money | Spend ledger with stacked daily bars; unknown amounts = 待补 |
| Map | Leaflet + OSM; dashed flights, solid ground; legs listed under the map |

Ship a usable shell even when many catalogs are empty: countdown/today, empty-state hints, unlock gate, FX card if a foreign currency is in play.

## Hard rules (from the Indonesia build)

- Built-in arrays (`prep`, `bags`, `spends`, `hotels`, …) are **catalogs only**. Couple add/edit/delete lives in `cloud.lists`. Never copy extras into catalogs. Never reset `cloud.lists` when changing a catalog.
- Cancelled **hotels stay listed** (`cancel` / `canceling`) so nobody walks into the wrong stay. Cancelled **orders leave the spend catalog**.
- TextDB writes: `POST https://api.textdb.online/update/` as `application/x-www-form-urlencoded`. Reads: `https://textdb.online/{id}` then API fallback. `cache: "no-store"` only — **no** `Cache-Control` / `Pragma` headers (CORS).
- Show last-sync time in **Beijing** (`Asia/Shanghai`). Store `updatedAt` as UTC ISO.
- Jump links use class `jump-ext` and must not fire while swiping tabs.
- Animate only inner `.pane-body`. Leaflet map: `isolation: isolate`.
- Custom spend categories get colors that do not reuse 机票/酒店/活动/交通/其他.
- FX amounts use the currency unit (e.g. `100 000盾`), not 万盾.
- Do not add long stale hint paragraphs (no visa essay box, no “这些日子换酒店勤”, no full “北京 → … → 北京” route sentence on the map).
- After UI work, verify in the browser (or the closest substitute) before calling it done.

## Build order

1. Intake (ask / leave empty).
2. Clone chrome from the Indonesia `index.html` if present; otherwise reconstruct from the blueprint.
3. Fill `START`/`END`, title, phases, `days`, then flights, hotels, acts, stops, ground legs, prep, bags, spends.
4. Wire unique `localStorage` keys (`trip{year}-{slug}-*`) so two assistants on one origin do not collide.
5. Preview locally, click 今日 → 行程四页 → 地图 → 准备三页, then commit.

## Later updates

Treat order screenshots and “改成住 X / 已退订 Y” as catalog patches. Keep day `stay` text, hotel `status`, spend rows, todos, and map pins in sync. Ask only for fields still missing after the screenshot.