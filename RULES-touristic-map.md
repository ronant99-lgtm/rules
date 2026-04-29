# RULES-touristic-map.md
# Interactive Touristic Map — Design System
# Version 1.0 · May 2026

---

## Overview

This rule file governs the creation of standalone HTML touristic map pages — travel guides with interactive Leaflet maps, category-filtered card lists, and popup markers. It defines the design language, layout structure, component library, and implementation patterns.

**Reference implementations:**
- `ny_map.html` (New York · Mai 2026)
- `memphis_guide_v2.html` (Memphis · Avril 2026)

---

## 1. Core Stack

- **Leaflet 1.9.4** for interactive maps — always load from `https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/`
- **CARTO Voyager** tile layer for the map (light, readable, neutral)
  ```
  https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png
  ```
- **Google Fonts:** `DM Serif Display` (italic variant included) + `DM Sans` (weights 300, 400, 600)
- **No external UI framework.** Pure CSS with CSS custom properties.

---

## 2. Typography

| Role | Font | Size | Weight |
|------|------|------|--------|
| Page title (H1) | DM Serif Display | 1.55rem | 400 |
| Card/popup title | DM Serif Display | 1rem / 15px | 400 |
| Body / desc | DM Sans | 0.8rem | 300–400 |
| Badge / kicker | DM Sans | 0.62–0.72rem | 600–700 |
| Address / meta | DM Sans | 0.72–0.74rem | 400 |

- Use `font-style:italic` on `<em>` inside the H1 for the place/date accent.
- Never use bold weight for DM Serif Display.

---

## 3. Color System

### Background & UI chrome
```css
body        { background: #f7f8fc; color: #1a1d2e; }
header      { background: linear-gradient(135deg, #1a1d2e 0%, #2d3561 100%); }
filter-wrap { background: #fff; border-bottom: 1px solid #e8eaf0; }
card        { background: #fff; }
```

### Category palette — CSS custom properties
Define one `--cat` color and one `--cat-bg` (10% opacity rgba) per category.

**Touristic category defaults:**

| Category | Color | Variable |
|----------|-------|----------|
| Food & Activities | `#FF6B6B` | `--food` / `--food-bg` |
| River / Water | `#5ba3d9` | `--river` |
| Native / Archaeological | `#b07d4a` | `--native` |
| Antebellum / Colonial | `#c9a84c` | `--ante` |
| Civil War / Military | `#d46a6a` | `--cwar` |
| Gilded Age / Architecture | `#c09ee0` | `--gilded` |
| Music / Culture district | `#e8a735` | `--beale` |
| Political / Civic era | `#7eb3e8` | `--crump` |
| Rock / Pop culture | `#e05c9f` | `--rock` |
| Civil Rights / History | `#4caf7d` | `--rights` |
| Suburbs / Rural | `#8bb89e` | `--suburb` |

**For day-based travel guides (NY model):**

| Day | Color |
|-----|-------|
| J0 | `#FF6B6B` |
| J1 | `#00B894` |
| J2 | `#0984E3` |
| J3 | `#6C5CE7` |
| J4 | `#F9A825` |
| J5 | `#E84393` |
| J6 | `#00CEC9` |

Extend this palette as needed for more days.

### Shadows
```css
--shadow:    0 2px 12px rgba(0,0,0,.08);
--shadow-md: 0 4px 20px rgba(0,0,0,.12);
```

---

## 4. Layout Structure

```
<header>               ← Sticky, z-index 1000
<stats-bar>            ← Optional key numbers
<advisory>             ← Optional warning banner  
<filter-wrap>          ← Sticky, z-index 999, top:80px (adjust if no stats-bar)
<map #map>             ← Full-width, 58vh, max 520px
<cards-section>
  <cat-group * n>
    <cat-header>       ← Sticky, z-index 100, top:127px (adjust per filter bar height)
    <cards-list>       ← 1 col mobile / 2 col 640px / 3 col 900px
```

**Sticky offsets** — recalculate if header height changes:
- Filter bar: `top: 80px` (header height ~80px)
- Category headers: `top: 127px` (header + filter bar ~47px)

---

## 5. Header Component

```html
<header>
  <div class="header-inner">
    <div class="header-title">
      <div class="header-kicker">🎸 Travel & History Guide · Context</div>
      <div class="header-h1">City, <em>State/Country</em></div>
      <div class="header-meta">Family name · Dates · Hotel or base</div>
    </div>
  </div>
</header>
```

- Kicker: uppercase, 0.65rem, 0.18em letter-spacing, `rgba(255,255,255,.55)` color
- H1 italic `<em>`: `#FDCB6E` (warm gold accent)
- Meta: 0.72rem, `rgba(255,255,255,.5)`

---

## 6. Filter Bar Component

```html
<div class="filter-wrap">
  <div class="filter-bar" id="filterBar">
    <button class="cat-btn all active" data-cat="all">All</button>
    <button class="cat-btn" data-cat="river"
      style="--cc:var(--river);--cb:var(--river-bg)">
      <span class="cat-dot" style="background:var(--river)"></span>River
    </button>
    <!-- ... -->
  </div>
</div>
```

- Inject `--cc` (color) and `--cb` (background) as inline CSS vars per button
- Active state uses `--cb` background, `--cc` text, `--cc` border
- `overflow-x: auto; scrollbar-width: none` — horizontally scrollable, no visible scrollbar
- `min-width: max-content` on inner `.filter-bar`

---

## 7. Map Component

### Tile layer
Always use CARTO Voyager (light). No dark mode for the map — the Voyager style reads well in all contexts.

### Marker icon factory
```javascript
function mkIcon(color, sz, num) {
  sz = sz || 28;
  var inner = num
    ? '<span style="color:#fff;font-size:'+(sz*.48)+'px;font-weight:700;line-height:1">'+num+'</span>'
    : '';
  return L.divIcon({
    className: '',
    html: '<div style="width:'+sz+'px;height:'+sz+'px;border-radius:50%;background:'+color+
          ';border:2.5px solid rgba(255,255,255,.95);'+
          'box-shadow:0 3px 10px rgba(0,0,0,.3),0 0 0 4px '+color+'30;'+
          'display:flex;align-items:center;justify-content:center">'+inner+'</div>',
    iconSize: [sz, sz],
    iconAnchor: [sz/2, sz/2],
    popupAnchor: [0, -sz/2-4]
  });
}
```

- Default size: 28px; food/activities: 30px; history: 26px
- Include sequential `num` counter per category so related markers are numbered
- The double ring glow (`0 0 0 4px ${color}30`) is a signature of this design system

### Popup override CSS
```css
.leaflet-popup-content-wrapper {
  border-radius: 14px !important;
  box-shadow: 0 8px 32px rgba(0,0,0,.18) !important;
  border: none !important;
  padding: 0 !important;
  overflow: hidden;
}
.leaflet-popup-content { margin: 0 !important; width: 260px !important; }
.leaflet-popup-tip-container { display: none; }
```

### Popup inner HTML structure
```javascript
'<div class="popup-inner">' +
  '<div class="popup-cat-badge" style="background:'+c+'18;color:'+c+'">'+LABEL+'</div>' +
  '<div class="popup-name">'+name+'</div>' +
  '<div class="popup-desc">'+desc+'</div>' +
  (url ? '<a href="'+url+'" class="popup-link" style="background:'+c+'18;color:'+c+'">🌐 Website</a>' : '') +
'</div>'
```

---

## 8. Card Component

```html
<div class="place-card" onclick="flyAndOpen('id')">
  <div class="card-accent" style="background:var(--cat-color)"></div>
  <div class="card-body">
    <div class="card-top">
      <div class="card-name">Place Name</div>
      <span class="card-badge-sm" style="background:var(--cat-bg);color:var(--cat-color)">Tag</span>
    </div>
    <div class="card-addr">Address · Hours</div>
    <div class="card-desc">Short description.</div>
    <div class="card-history">Historical context. (italic, for history cards)</div>
    <div class="card-note">Personal/insider tip. (yellow left border)</div>
    <div class="card-footer">
      <a class="card-web-link" href="..." style="background:var(--cat-bg);color:var(--cat-color)">🌐 Website</a>
      <button class="card-map-btn" onclick="event.stopPropagation();flyAndOpen('id')">📍 Map</button>
    </div>
  </div>
</div>
```

**Card accent stripe:** 5px wide left border using the category color — the visual anchor of each card.

**`.card-note`** (insider tip): `background:#fef9f0; border-left:2px solid #FDCB6E`. Prepend `💬 `.

**Card grid:**
- Mobile: 1 column
- ≥640px: 2 columns
- ≥900px: 3 columns

---

## 9. Category Group Component

```html
<div class="cat-group" data-cat="river">
  <div class="cat-header">
    <div class="cat-header-pill" style="background:var(--river-bg);color:var(--river)">
      🌊 The River
    </div>
    <div class="cat-header-label">Where Memphis began</div>
  </div>
  <div class="cards-list">
    <!-- cards -->
  </div>
</div>
```

- `data-cat` must match the value used in filter buttons and `PLACES` data
- Category header is sticky to allow scrolling within a long section

---

## 10. flyAndOpen Function

```javascript
function flyAndOpen(id) {
  var m = markers[id];
  if (!m) return;
  var ll = m.getLatLng();
  map.flyTo(ll, 15, {duration: 0.8});
  setTimeout(function(){ m.openPopup(); }, 900);
  document.getElementById('map').scrollIntoView({behavior:'smooth', block:'start'});
}
```

---

## 11. Filter Logic

```javascript
function applyFilter(cat) {
  // 1. Show/hide markers
  allMarkers.forEach(function(m) {
    if (cat === 'all' || m._catKey === cat) {
      if (!map.hasLayer(m)) map.addLayer(m);
    } else {
      if (map.hasLayer(m)) map.removeLayer(m);
    }
  });
  // 2. Show/hide card groups
  document.querySelectorAll('.cat-group').forEach(function(g) {
    g.style.display = (cat === 'all' || g.getAttribute('data-cat') === cat) ? '' : 'none';
  });
  // 3. Fit bounds to visible markers (or reset)
  if (cat !== 'all') {
    var pts = PLACES.filter(function(p){ return p.cat === cat; }).map(function(p){ return [p.lat,p.lng]; });
    if (pts.length) map.flyToBounds(pts, {padding:[50,50], duration:0.7, maxZoom:15});
  } else {
    map.flyTo([DEFAULT_LAT, DEFAULT_LNG], DEFAULT_ZOOM, {duration:0.5});
  }
}
```

For **day-based** guides (NY model), replace `cat` with `day` and `_catKey` with `_dayKey`. Logic is identical.

---

## 12. Data Structure

```javascript
var PLACES = [
  {
    id:   'unique_id',      // kebab-case, matches markers{} key
    cat:  'river',          // or 'day' key for day-based guides
    lat:  35.145,
    lng:  -90.055,
    name: 'Place Name',
    desc: 'Short description for popup.',
    url:  'https://...'     // null if no website
  },
  // ...
];
```

Build `markers{}` lookup object during init: `markers[p.id] = leafletMarker`.
Tag each marker: `m._catKey = p.cat` (or `m._dayKey = p.day`).

---

## 13. Advisory / Warning Banner

Place between stats bar and filter bar. Styled as a full-width band:

```html
<div class="advisory">
  <span class="advisory-icon">⚠️</span>
  <div><strong>Area to avoid</strong> — Message here.</div>
</div>
```
```css
.advisory {
  padding: .75rem 1rem;
  background: #fff3cd;
  border-bottom: 1px solid #ffc107;
  font-size: .78rem;
  color: #856404;
  display: flex;
  align-items: flex-start;
  gap: .5rem;
}
```

---

## 14. Stats Bar (optional)

A row of 3–5 numbers above the filter bar:

```html
<div class="stats-bar">
  <div class="stat"><div class="stat-num">55+</div><div class="stat-label">Historic Sites</div></div>
  <!-- ... -->
</div>
```
```css
.stats-bar { display:flex; background:#fff; border-bottom:1px solid #e8eaf0; }
.stat { flex:1; text-align:center; padding:.8rem .5rem; border-right:1px solid #e8eaf0; }
.stat-num { font-family:'DM Serif Display',serif; font-size:1.5rem; color:#1a1d2e; }
.stat-label { font-size:.62rem; color:#9ca3af; text-transform:uppercase; letter-spacing:.08em; }
```

---

## 15. Scroll-to-top Button

```html
<button id="scrollTop" onclick="window.scrollTo({top:0,behavior:'smooth'})">↑</button>
```
```javascript
window.addEventListener('scroll', function(){
  document.getElementById('scrollTop').classList.toggle('show', window.scrollY > 300);
}, {passive:true});
```

Position: `fixed; bottom:1.2rem; right:1rem;` — dark navy circle, appears after 300px scroll.

---

## 16. Responsive Breakpoints

| Breakpoint | Effect |
|------------|--------|
| Default (mobile) | 1-col cards, map 58vh |
| ≥640px | 2-col cards, filter sticky top adjusted |
| ≥900px | 3-col cards, map up to 62vh / 620px max |

---

## 17. Print Styles

```css
@media print {
  .filter-wrap, .map-section, #scrollTop, .stats-bar { display: none; }
  .place-card { box-shadow: none; border: 1px solid #e5e7eb; }
}
```

---

## 18. Do / Don't

**Do:**
- One `PLACES` array as the single source of truth — drives both markers and cards
- Use `flyAndOpen(id)` consistently from both card `onclick` and map-pin buttons
- Apply `stopPropagation()` on nested buttons inside cards
- Reset category counters per category to get sequential numbered markers
- Include `html lang` attribute matching the guide's language

**Don't:**
- Don't use a dark tile layer (CARTO Dark_all) — it hurts readability on mobile in daylight
- Don't mix day-based and category-based filtering in the same guide
- Don't hardcode popup HTML with `background-color` — use `${color}18` opacity trick
- Don't use `localStorage` for theme persistence on simple travel guides (no dark mode needed)
- Don't put `max-width` containers on the map or filter bar — both must be 100% viewport width
- Don't forget `event.stopPropagation()` on any button nested inside a card that has `onclick`

---

## 19. File Naming Convention

```
[city_slug]_[year].html        — e.g. memphis_2026.html, new_york_2026.html
[city_slug]_guide.html         — generic version
```

---

## 20. Checklist Before Delivery

- [ ] `lang` attribute set correctly (fr / en)
- [ ] All `PLACES` IDs are unique and match `markers{}` keys
- [ ] All `data-cat` values on `.cat-group` elements match `PLACES[].cat` values exactly
- [ ] `flyAndOpen` tested for all referenced IDs
- [ ] Filter "All" resets to correct default map center and zoom
- [ ] `map.flyToBounds` called on category filter (not just `flyTo`)
- [ ] Advisory banner present if safety notes exist
- [ ] Print stylesheet hides map and filter bar
- [ ] Scroll-to-top button functional
- [ ] Mobile layout tested (single column cards, horizontal filter scroll)
