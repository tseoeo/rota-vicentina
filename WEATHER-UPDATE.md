# Weather Update Instructions

This guide explains how to update the weather forecast in the Rota Vicentina trip guide.

## Files to update

Both files live in `/Users/ivandimitrov/Projects/rota-vicentina/`:
- `itinerary.html` — day-by-day guide with weather cards
- `map.html` — interactive map with color-coded trail and popup weather

The source files (not the ones served by GitHub Pages) are in:
`/Users/ivandimitrov/Projects/rota vicentina/research/rota-vicentina-guide/`
**Always edit the ones in `/Users/ivandimitrov/Projects/rota-vicentina/` directly** — that's the git repo that publishes to GitHub Pages at `tseoeo.github.io/rota-vicentina`.

## Trip details

| Day | Date | Route | Overnight |
|-----|------|-------|-----------|
| 1 | Tue 15 Apr | Porto Covo → Vila Nova de Milfontes | Vila Nova de Milfontes |
| 2 | Wed 16 Apr | VN Milfontes → Almograve | Almograve |
| 3 | Thu 17 Apr | Almograve → Zambujeira do Mar | Zambujeira do Mar |
| 4 | Fri 18 Apr | Zambujeira → Odeceixe | Odeceixe |
| 5 | Sat 19 Apr | Odeceixe → Aljezur | Aljezur |
| 6 | Sun 20 Apr | Aljezur → Praia da Arrifana | Vale da Telha / Arrifana |
| 7 | Mon 21 Apr | Arrifana → Carrapateira | Carrapateira |
| 8 | Tue 22 Apr | Carrapateira → Vila do Bispo | Vila do Bispo |
| 9 | Wed 23 Apr | Vila do Bispo → Sagres | Sagres |
| 10 | Thu 24 Apr | Sagres → Praia da Salema | Salema |
| 11 | Fri 25 Apr | Salema → Lagos | Lagos (finish) |

## Step 1 — Research the forecast

Search multiple sources for each location and date. Good sources:
- timeanddate.com (14-day extended forecast for Sagres and Algarve)
- yr.no (Sagres, Zambujeira do Mar, Vila do Bispo)
- meteoblue.com (Vila Nova de Milfontes)
- ipma.pt (Portuguese national weather service)
- accuweather.com

For each day, collect:
- High / low temperature (°C)
- Weather condition + emoji (☀️ sunny, ⛅ partly cloudy, ☁️ overcast, 🌦️ showers, 🌧️ rain)
- Precipitation probability (%)
- Wind speed (km/h) and direction
- UV index

## Step 2 — Update itinerary.html

### Weather cards (one per day)

Each day section has a `<div class="weather-card">` right after the `<div class="day-stats">` closing tag. Update the three spans:

```html
<div class="weather-card">
  <span class="weather-icon">☀️</span>
  <span class="weather-temp">22–26°C / 11°C</span>
  <span class="weather-detail">Sunny · 0% rain · Wind NW 15 km/h · UV 7 · ↑06:58 ↓20:07</span>
</div>
```

### Practical Info weather card

Near the top of the file, find `<h4>Weather</h4>` inside `<section class="practical">`. Update the paragraph with a plain-English summary covering:
- Which days are best
- Which days have shower risk
- Any wind warnings
- UV note

Also update the date in the disclaimer line:
```html
<p class="weather-disclaimer">Forecast as of [DATE]. Check <a href="https://www.ipma.pt" target="_blank">IPMA.pt</a> for latest updates.</p>
```

## Step 3 — Update map.html

### Trail colors

The `trailColors` array (one entry per day, index 0–10) uses this weather color scheme:

| Color | Hex | When to use |
|-------|-----|-------------|
| Gold | `#eab308` | Sunny / dry (0–10% rain) |
| Grey | `#6b7280` | Overcast (10–30% rain) |
| Orange | `#f97316` | Showers (30–55% rain) |
| Red | `#dc2626` | Rain + wind warning (55%+ or strong wind) |

### Trail segment popups

Each polyline ends with `.bindPopup("...").addTo(layers.trail)`. Update the popup string for each day:

```javascript
bindPopup("<strong>Day 1 — Tue 15 Apr</strong><br>Porto Covo → VN Milfontes<br><span style='font-size:0.82rem'>☀️ 22–26°C / 11°C &middot; 0% rain &middot; NW 15 km/h &middot; UV 7</span>").addTo(layers.trail)
```

### Overnight stop popups

The `overnights` array has a weather line in each entry. Update the `<span>` inside each string:

```javascript
[37.7267, -8.7828, 'Vila Nova de Milfontes', 'Night 1 — Casa das Marias<br><span style="font-size:0.8rem">☀️ 22–26°C / 11°C · 0% rain · NW 15 km/h · UV 7</span>'],
```

### Weather overlay markers

The `weatherData` array drives the small floating labels (toggled via the "Weather" checkbox in the filter panel). Update `icon`, `high`, `low`, `rain`, and `wind` for each entry.

## Step 4 — Commit and push

```bash
cd /Users/ivandimitrov/Projects/rota-vicentina
git add itinerary.html map.html
git commit -m "Update weather forecast — [DATE]"
git push origin main
```

GitHub Pages rebuilds in ~1–2 minutes. The live site is at:
**https://tseoeo.github.io/rota-vicentina**
