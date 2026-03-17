# CongQ's Finance Toolkit — Claude Context

## Project Overview
A single-page personal finance toolkit (`index.html`) deployed on Firebase Hosting. All app code (HTML, CSS, JS) lives in one file. Firebase Firestore is used for cloud sync of user data.

**Live site:** https://congq-finance-toolkit.web.app

## Version Tracking — REQUIRED on every change
Every time `index.html` is modified, update the version string near line 920:
```html
<div class="sidebar-footer">v.XXXXX</div>
```
Generate a new random 5-character alphanumeric string (a-z, 0-9). No exceptions — applies to any CSS, HTML, or JS change.

## Deploy Workflow
```bash
# Commit & push
git add index.html
git commit -m "description v.XXXXX"
git push

# Deploy to Firebase
firebase deploy --only hosting
```
Remote is HTTPS with a personal access token embedded in the URL (already configured).

## Architecture Notes

### Single File (`index.html`)
- All CSS in `<style>` block
- Firebase SDK loaded as `type="module"` (separate script tag at top)
- All app JS in a regular `<script>` block
- Firebase helpers exposed to global scope via `window._fsSet`, `window._fsGet`, etc.

### Live Action Dashboard
Located at the bottom of the JS section. Key constants and patterns:
- `LIVE_ACTION_LS = 'live-action-v1'` — localStorage cache key
- `LIVE_ACTION_TTL = 15min` — fast source refresh interval
- `LIVE_ACTION_SLOW_TTL = 24h` — slow source refresh interval
- `LA_SOURCES` — registry of all data sources; each has `ids`, `fetch`, and optionally `slow: true` + `tsKey`
- **Slow sources** (Fed Funds Rate, US 1Y/10Y/3M Treasury): skipped during regular refresh, show "Xm/h/d ago · ↻ Refresh" per tile
- **Fast sources**: refreshed every 15 min automatically
- `liveActionRender(d)` — rebuilds all tile HTML from cached data object `d`
- `_laUpdateTile(id, val, sub, color)` — live DOM update for a single tile during fetch

### Data Sources Used
- Yahoo Finance (`query1/query2.finance.yahoo.com`) via CORS proxy — equities, crypto, commodities, some bonds
- NY Fed EFFR API (`markets.newyorkfed.org`) — Fed Funds Rate (direct, no proxy)
- FRED CSV (`fred.stlouisfed.org`) via CORS proxy — fallback for Fed rate, US 1Y Treasury (DGS1)
- open.er-api.com — Forex spot rates
- Alternative.me — Fear & Greed index
- CORS proxies: allorigins.win (primary), corsproxy.io (fallback)

## Design System
Warm parchment theme. CSS variables defined in `:root`:
- `--bg: #F5F0E8`, `--text: #2C2416`, `--blue: #C17F3A` (accent), `--green: #3DAA6A`, `--red: #D44A30`
- Glass morphism panels: `--glass-bg`, `--glass-blur`, `--glass-shadow`
- Fonts: Inter (UI), Fraunces (serif headings)
