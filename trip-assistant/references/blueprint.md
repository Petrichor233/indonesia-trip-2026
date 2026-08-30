# Blueprint (match the Indonesia assistant)

Canonical implementation: the Indonesia `index.html` in this family of projects. Copy chrome and behavior; swap catalogs.

## Shell

```
header          eyebrow + 中文标题 + 日期 · N 天
#subchrome      sticky sub-tabs for 行程 / 准备
#page-today
#page-trip
#page-map
#page-prep
nav             今日 行程 地图 准备
```

Mobile first, max-width 720px on desktop. Theme: `--accent #0d9b86`, purple / orange / blue chips, gradient header. Leaflet 1.9.x from jsDelivr.

## Information architecture

**今日** — before trip: countdown + first day. During: date title, phase chip, stay/breakfast/flight meta, 下一步, 当天安排, 明天早起, 时差, 汇率. After: 到家了.

**行程 / 日程** — filter chips by phase. Day buttons expand a card: stay, breakfast, flight, timezone, timed events (`must` / `hl`). Tap date/name jumps across tabs.

**行程 / 航班** — ticket cards: times, duration (from airport UTC offsets if you have them), bag, 飞常准 jump.

**行程 / 酒店** — every stay including `cancel` and `canceling`. Chip: 已确认 / 待预订 / 已退订 / 待退订. Qunar (or platform) jump only when `platform` matches.

**行程 / 活动** — photo wash optional; Klook jump when `klook:true`.

**地图** — no long “A → B → C” sentence. Focus buttons per phase. Legend: 虚线航班, 实线陆路/轮渡, 细虚线船游, 小点酒店. Under the map: **包车 / 轮渡** rows (date · kind · 约 km · 约 duration). Then 途经点.

**准备 / 待办** — 行前进度 bar; groups 行前必办 / 按日期 / 自加; 共享备忘 textarea at the bottom. Unlock to edit.

**准备 / 行李** — three-state cycle 尚无 → 已有 → 已装包.

**准备 / 花费** — stats 已记账 / 待补 / 全部; 按类目 single-color bars; 按日期 **stacked** by category; legend; detail list with add/edit/delete.

## Catalogs (built-in only)

Do not put user extras here.

```text
flights[]   date iso no al route from to dep arr bag book note tz
hotels[]    inn out nights name aka[] room status platform note lat lng
            status: ok | wait | cancel | canceling
acts[]      date name aka[] who detail note klook img
days[]      date iso title phase stay breakfast flight tz
            events[]: t title note kind tz
bags[]      id g t st          st: none | have | packed
prep[]      id g t done
spends[]    id cat date name cny|idr note
stops[]     id name lat lng phase date desc
flightSegs  [fromStopId, toStopId]
groundSegs  [fromStopId, toStopId]
GROUND_LEGS date from to kind km dur
```

Phases: 3–4 trip regions + `transit`. Map `phaseName` / `PHASE_COLOR` / chip classes to those keys (Indonesia used komodo / bali / java / transit).

Spend categories fixed: `机票 酒店 活动 交通 其他`. Built-in colors:

```text
机票 #8b5cf6   酒店 #0d9b86   活动 #e67e22   交通 #3b82f6   其他 #64748b
```

Custom cats: `#e11d48 #65a30d #0891b2 #c026d3 #92400e #4f46e5 #f59e0b #155e75` in order, never reuse the five above.

Unknown money: `cny:null` (and/or `idr:null`) → UI 待补. FX: `https://open.er-api.com/v6/latest/CNY`. Display foreign cash as `100 000盾` style (space thousands + unit), not 万盾.

## Sync (do not “improve” this)

```text
cloud = { prep, pack, notes, pinHash, updatedAt, by, lists }
lists = extraPrep extraPack extraSpend
        gonePrep gonePack goneSpend
        editPrep editPack editSpend
```

- Share id: `td` + 16 hex. URL `?sid=`. PIN ≥ 4, unlock is session-only (`trip…-unlock`).
- Local draft `trip…-draft`. Serialize writes. Skip poll while `dirty || pushing`.
- Write: `POST https://api.textdb.online/update/` body `key` + `value` (JSON of payload).
- Read: `https://textdb.online/{id}?_={ts}` then `https://api.textdb.online/{id}?_={ts}`.
- `fetch(..., { cache: "no-store" })` only. No extra headers.
- `fmtSyncTime` in `Asia/Shanghai`. Gate copy: 解锁编辑 / 改为只读; 换浏览器用「复制链接」, not the bare homepage.
- `pagehide` may `sendBeacon` the same update URL.

Merge: `livePrep` / `liveBags` / `liveSpend` = catalog + extras − gone + edits.

## Chrome behavior

- Sticky `#subchrome`; swipe 行程 and 准备 sub-tabs; animate only `.pane-body`.
- `a.jump-ext`: `pointer-events:none` while `.wrap.swiping`; swallow click after swipe.
- Header and map: `isolation: isolate` so Leaflet/stacking stay sane.
- Internal jumps: inherit text color, underline, no “button” look.
- Unlock on 准备 becomes 「改为只读」 while `canEdit`.

## Empty and pending copy

Short operational hints only. Prefer a 待办 row over a static 签证/入境 essay. Prefer a `wait` hotel over hiding the night. Map route sentence stays deleted.

## Verification

Exercise 今日, 行程 (all four sub-tabs), 地图 (focus + legs), 准备 (all three). Check empty states and one stacked spend day if any day has two categories. If you change layout or state, click the other surfaces that read it.