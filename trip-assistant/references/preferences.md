# Preferences (from the Indonesia trip-assistant work)

Apply these unless the user overrides them.

## Product

- Two people, shared truth. Edits must survive a second browser via TextDB, not only `localStorage`.
- Companion link is `?sid=td…` after 复制链接. The public homepage alone is view / local-draft.
- Catalogs are the published plan. `cloud.lists` is the couple's overlay. Do not flatten extras into source.
- Keep wrong or cancelled bookings visible on 酒店 (pink 待退订 / grey 已退订) and say 勿入住 + where they actually stay. Delete those orders from 花费 once fully cancelled.
- Duplicate bookings stay on the hotel list even if the itinerary sleeps elsewhere.
- Order screenshots win over older catalog amounts, room names, and platforms.

## Copy

- Chinese, short, concrete. Times, order ids, who picks up.
- Do not add filler that goes stale: no “已买行李额会显示完成”, no “换酒店勤的日子：…”, no map-wide “北京 → 胡志明 → … → 北京”.
- Visa/entry facts belong in 待办 rows (e-VOA, MDAC), not a footer textbook.
- 待补 / 待预订 / 需预约 are fine. Do not invent a price or a hotel to look complete.
- Timezone label is **时差**, not a lecture. Sync clock is Beijing.

## UI

- Feishu-inspired cards and teal accent; do not clone Feishu layout or content.
- Stacked **按日期** spend bars by category; custom categories must not reuse built-in colors.
- 按类目 keeps one color per row plus a matching dot.
- Ground/ferry **约** km and duration sit under the map, not as a paragraph above it.
- Activity photos: full-card image + soft diagonal wash; preload when present.
- Hotel map dots omit `cancel` / `canceling`.

## Workflow

- Ask when the plan is incomplete; leave gaps; fill when they come back with a shot or a one-liner.
- After any UI or itinerary change, click through the real tabs. A single screenshot is not enough.
- Same-origin second trip: new `localStorage` key prefix so drafts do not clobber the Indonesia app.
- Do not “fix” TextDB by adding cache headers.

## Tone when talking to this user

Lead with what changed. Use their place names (大丰收, Purwa, 库塔). Do not pad with timelines or policy quotes.