# CongQ's Finance Toolkit

A personal, single-page financial dashboard built with vanilla HTML/CSS/JS and deployed on Firebase.

**Live:** https://congq-finance-toolkit.web.app

## Features

- **Live Action** — Real-time market dashboard covering equities, crypto, commodities, forex, and macro indicators. Data refreshes every 15 minutes with smart caching; slow-changing data (interest rates) cached for 24h with manual refresh.
- **Feed** — Curated crypto/macro news with AI-powered sentiment analysis.
- **Loan Calculator** — Amortization schedule with extra payment modeling.
- **Delta Neutral Positions** — Track and manage delta-neutral crypto trading positions.
- **Cloud Sync** — All user data synced via Firebase Firestore across devices.

## Tech Stack

- Pure HTML/CSS/JS — no build step, single `index.html`
- Firebase Hosting + Firestore
- Data sources: Yahoo Finance, NY Fed, FRED, open.er-api.com, Alternative.me

## Deploy

```bash
firebase deploy --only hosting
```
