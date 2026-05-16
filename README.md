# SimFly Relationship Manager Lite

A single-file relationship-tracking tool for SimFly pilots. Download it, open it in any browser, and start tracking who's been flying to your airports — so you know who you owe a return visit to. No installation, no server, no account, no cloud sync. Everything lives locally on your machine.

This app pairs with [SimFly Active Airports](https://github.com/KyndraRocks/SimFlyActiveAirports) for flight planning — set up a route in RM Lite, click Launch, and AA opens in a new tab ready to file on SimBrief.

**Current version: v1.0.1**

---

## Download

Grab the latest release from the [Releases](https://github.com/KyndraRocks/SimFly-Relationship-Manager/releases) page. Download `SimFly_Relationship_Manager_Lite-LATEST.html`, open it in your browser, and you're ready to go.

---

## First-run setup

When you first open the app, a welcome modal walks you through three quick steps:

1. **Pick a save file** — click **📁 Choose save file…** in the sticky top bar to pick a JSON file on disk. The app autosaves to it every time you make a change. Chrome / Edge only (File System Access API). In other browsers, use **⬇ Export** for manual backups.
2. **Point at your Active Airports file** — click **✈ Set AA file…** in the sticky top bar and paste the path or URL to your local `SimFly_Active_Airports-LATEST.html`. Required for the Launch and Send to AA buttons. Get Active Airports from the [SimFlyActiveAirports releases](https://github.com/KyndraRocks/SimFlyActiveAirports/releases).
3. **Bring in your flights** — type pilots in one at a time via **Quick Entry**, or use the **↓ Sync Inbound Flights from SimFly** panel to bulk-import everything via a DevTools console snippet on simfly.io.

---

## Features

### Balance Queue
The main view — a divergent bar chart showing every tracked pilot's **inbound visits to you** (right, cyan) vs **your outbound returns to them** (left, green). No row limit; all pilots are shown.

Click a pilot row to expand it: their airport ICAOs appear as clickable chips, alongside **✈ DEP**, **✈ ARR**, **↩ Recip**, and **✎ Edit** buttons. Column headers sort the list — click again to reverse. A search box narrows by pilot name, 3-letter code, or airport ICAO. Date-range presets (3d / 7d / 14d / 30d / 90d / 1yr / All) plus custom from/to filters narrow both the chart and the row count. Two checkboxes — **Hide inactive** and **Has airports** — further trim the view.

A **☐ Select for AA** toggle enters selection mode: tick the pilots you want to focus on, then **🗺 Send to AA →** opens Active Airports with the region map filtered to just those pilots' airports.

### Quick Entry & Sync Inbound Flights
For one-off entries: type a pilot's name or 3-letter code, press Enter, done. The session log below each entry shows everything you've added with an Undo button on every row.

For bulk import: open the **↓ Sync Inbound Flights from SimFly** panel. Step 1 is a DevTools console snippet you paste in simfly.io's console — it walks your PAX wallet back to your last sync and copies a JSON list of new inbound flights to a textarea on the page. Step 2 is pasting that JSON back into RM Lite and clicking Import. New pilots are auto-created with a 3-letter code derived from their username; duplicates are skipped.

A **cutoff** row is automatically maintained — the most recent flight you've synced, so the next sync knows where to resume.

### Pilot Directory
Manage SimFly profile URLs for every tracked pilot and every airport owner in one place. The **Track →** button promotes a known owner (someone with airports in the community database) to a tracked pilot you can score in the Balance Queue.

### Recip Dashboard
A detailed history view with date-range presets at the top:

- **Summary chips** — outbound returns, pilots reciprocated, distinct destinations, inbound visits, pilots visiting you, last visit dates
- **↩ Top Pilots Visited** — horizontal bar chart, top 20 by reciprocation count
- **📍 Top Destinations** — top 20 destination ICAOs with their owning pilot
- **↩ Reciprocation List** — every return flight, filterable by pilot, sortable, exportable as CSV
- **⤓ Backfill from SimFly** — collapsible at the bottom for seeding historical reciprocations using a console snippet against simfly.io's `/api/user/flights` endpoint. Safe to re-run.

### Launch to Active Airports
Click **✈ Dep** or **✈ Arr** on any pilot row to set them as departure or arrival. A **Route banner** appears at the top showing the route and an **✈ Open in Active Airports** button. Clicking it opens your local AA file in a new tab pre-loaded with the route.

The first time you click Launch, the app asks where your AA file lives — paste the path (or http(s):// URL if you serve AA from a local web server) and it's saved. The **✈ Set AA file…** button on the sticky top bar lets you change it anytime.

### Three-tier local storage
Your data lives in three places:

1. **In memory** — live, mutated freely as you work.
2. **localStorage** — synchronously written on every change. Your safety net if everything else fails.
3. **A local disk file** — if you've connected one via **📁 Choose save file…**, the app autosaves a JSON snapshot to that file every change (debounced ~2 seconds). Requires the File System Access API (Chrome / Edge).

No GitHub PAT, no cloud sync, no telemetry, no external storage. The only network requests the app ever makes are to the public airport / runway / community-gist data sources listed below.

### Export / Import
Always available, every browser:

- **⬇ Export** — download a dated JSON snapshot of all your data. Use this for periodic backups, cross-device transfer, or as a fallback in browsers without File System Access support.
- **⬆ Import** — load any JSON snapshot. Choose **Merge** (union by ID and timestamp; nothing in your current data is lost) or **Replace All** (discard current data, file becomes the source). Confirms before either path.

### Inline help
A **?** button in the toolbar opens the full guide. The first-run welcome modal can be re-opened by clearing `localStorage['simfly_rmlite_welcome_seen']` and reloading.

---

## Data Sources

- **Airport database** — [OurAirports](https://ourairports.com) (open data, fetched at runtime)
- **Pilot-owned airports** — public community Gist maintained by the SimFly Active Airports project
- **Airport categories** — public community Gist maintained by the SimFly Active Airports project

---

## Usage Notes

- Works entirely in the browser. The only data that leaves your machine is the OurAirports/community-gist fetches above.
- Tested in Chrome and Edge. Safari and Firefox are generally compatible, but autosave-to-disk requires Chrome or Edge (other browsers use Export/Import only).
- The HTML file can be saved locally and used offline — but the airport database fetch needs an internet connection on first load.
- Companion app: [SimFly Active Airports](https://github.com/KyndraRocks/SimFlyActiveAirports) — required for the Launch / Send to AA features.
