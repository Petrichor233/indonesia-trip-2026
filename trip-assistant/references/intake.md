# Intake

Ask in batches. If the user does not have something, write 待预订 / 待补 / empty and move on.

## Must-have (ask first)

Enough to title the app and draw a day list:

| Field | If missing |
|---|---|
| Destination / trip name | Ask. Example: 「日本关西 11 天」 |
| Start and end dates | Ask. Need at least a start date to build `days[]`. |
| Who | Solo / couple / more. Default couple sync like the Indonesia app. |
| Home city and home timezone | Needed for 今日 + 时差. Default 北京. |
| UI language | Default 中文. |

Do not generate a filled itinerary with only a destination and no dates. You may still scaffold chrome and one empty day after they confirm they want a blank shell.

## Optional (ask once, then leave empty)

Group as: 航班 · 住宿 · 每天在干什么 · 活动 · 花费 · 地图.

| Topic | Empty representation |
|---|---|
| Flights | `flights = []`. 航班页 hint: 还没填航段. |
| Hotels | One `wait` row per night if dates exist but no hotel name; or `hotels = []` with hint 待补. |
| Day-by-day events | Each `days[]` entry exists for the date range; `events` can be one `{ t:"全天", title:"行程待补", note:"", kind:"must" }`. |
| Activities | `acts = []`. |
| Packing | Keep the Indonesia-style groups (证件 / 电子 / 衣物 / 鞋 / 洗护 / 药品 / 潜水或户外 / 其他) and trim items that do not apply. Do not invent trip-specific gear. |
| Todos | Seed only what the brief implies (签证、保险、接送). No essay boxes. |
| Spend | Seed from known orders only. Unknown booked items: `cny:null` or `idr:null` and 待补. |
| Currency / FX | If they will pay in a foreign cash, add 今日汇率速算. Otherwise omit the FX card. |
| Timezones vs Beijing | If all stops match Beijing, skip the 时差 card or say 同北京. |
| Map points | Stops from cities they named. No fake lat/lng. If unknown, omit the pin and still list the name in 途经点. |
| Ground / ferry legs | Only legs they mentioned. Distance/duration: look up typical values and label 约; if unknown, leave `dur` / `km` as 待补. |
| Jump links | 飞常准 / Klook / 去哪儿 / 携程 only if they use those platforms. |
| Couple share | Default on (TextDB + PIN). If solo and they refuse sync, keep the gate card but they can skip 创建共享. |
| Activity photos | Optional `img/acts/*.webp`. No photo is fine. |

## Screenshot / order updates

When they send a booking image, extract: name, dates, room, platform, order id, amount, breakfast, cancel policy, guest. Patch hotel + spend + the day's `stay` text together. Ask only for fields the image does not show.

## First reply template

Use a short checklist, then wait if must-haves are missing:

```
先对齐这几项（没有的就说「暂时没有」，我会留空）：
1. 目的地和旅行名称
2. 出发/返回日期
3. 几个人，从哪出发
4. 已订航班（可不填）
5. 已订酒店（可不填）
6. 每天大致安排（可不填）
7. 要不要两人同步（默认要）
```

After they answer, restate what will be filled vs left 待补, then build.