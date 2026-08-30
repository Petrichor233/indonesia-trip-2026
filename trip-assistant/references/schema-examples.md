# Catalog stubs

Use when starting a trip with dates but little else. Replace placeholders; do not ship Indonesia data.

```javascript
const START = new Date(2026, 10, 1); // month is 0-based
const END = new Date(2026, 10, 7);
const KEY_WHO = "trip2026-kansai-who";
const KEY_SID = "trip2026-kansai-sid";
const KEY_UNLOCK = "trip2026-kansai-unlock";
const KEY_FX = "trip2026-kansai-fx";
const KEY_DRAFT = "trip2026-kansai-draft";
```

```javascript
const days = [
  {
    date: "11.1", iso: "2026-11-01", title: "出发", phase: "transit",
    stay: "住宿待补", breakfast: "待定", flight: "航班待补",
    events: [{ t: "全天", title: "行程待补", note: "", kind: "must" }]
  }
];

const hotels = [
  { inn: "11.1", out: "11.2", nights: 1, name: "当晚住宿待补", aka: [],
    room: "待定", status: "wait", platform: "", note: "待预订", lat: null, lng: null }
];

const flights = [];
const acts = [];
const spends = [];
const GROUND_LEGS = [];
```

Hotel without coordinates: omit `lat`/`lng` or set `null`. `mappedHotels()` already skips missing coords and cancelled stays.

Spend row with no amount:

```javascript
{ id: "sp-h-1", cat: "酒店", date: "11.1", name: "当晚住宿", cny: null, note: "待补" }
```
