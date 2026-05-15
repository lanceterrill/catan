# 🎲 Catan Dice Game — Lance & Kat

A mobile-first digital scoresheet for the **Catan Dice Game (Island One)**, built for two players. Tracks scores across 15 rounds, saves game history to Google Sheets, and includes an interactive island map and full rules reference — all in a single HTML file with no dependencies.

**Live:** [lanceterrill.github.io/catan/](https://lanceterrill.github.io/catan/)

---

## Features

- **Mobile-first scoring** — tap +/− buttons to score each round; no keyboard required
- **Player tabs** — Lance and Kat each get their own panel on mobile; side-by-side columns on desktop
- **Live scoreboard strip** — running totals pinned at the top on mobile, winner highlighted in real time
- **Google Sheets history** — save completed games (date, scores, winner, rounds played) to a shared spreadsheet; view full history with win tallies in-app
- **Island One map** — accurate hex grid with all 15 road segments numbered in build order, settlements, cities, and knights labeled with point values and joker resources
- **Rules reference** — complete How to Play section covering rolling, building costs, knights, bonuses, and scoring
- **Print-ready** — game table prints cleanly; map, rules, history, and controls are hidden
- **No install, no build step** — one HTML file, works from any browser

---

## Setup

### 1. Deploy to GitHub Pages

The file is named `index.html` and lives in the `/catan/` repository (or folder). GitHub Pages serves it automatically at `lanceterrill.github.io/catan/`.

### 2. Connect Google Sheets (for game history)

Game history requires a Google Apps Script backend. One-time setup, takes about 5 minutes.

**A. Create the spreadsheet**

1. Go to [sheets.google.com](https://sheets.google.com) and create a new spreadsheet
2. Name it anything — **Catan Scores** works well

**B. Add the Apps Script**

1. In the spreadsheet, click **Extensions → Apps Script**
2. Delete the default `myFunction` code
3. Paste the contents of `catan-apps-script.js` (included in this repo)
4. Click **Save** (Ctrl+S / ⌘S)

**C. Deploy as a Web App**

1. Click **Deploy → New deployment**
2. Click the gear icon next to "Type" → select **Web app**
3. Set:
   - **Execute as:** Me
   - **Who has access:** Anyone
4. Click **Deploy**
5. Copy the **Web App URL** (looks like `https://script.google.com/macros/s/.../exec`)

**D. Wire the URL into the scoresheet**

Open `index.html` and find this line near the top of the `<script>` block:

```js
const SHEETS_URL = 'YOUR_WEB_APP_URL_HERE';
```

Replace the placeholder with your copied URL:

```js
const SHEETS_URL = 'https://script.google.com/macros/s/ABC123.../exec';
```

Save and push to GitHub. History is now live on both phones.

> **Note:** The Apps Script creates a `Games` sheet automatically on first save with styled headers. You don't need to set up columns manually.

---

## File Structure

```
/catan/
├── index.html            # The entire app — scoresheet, map, rules, history UI
├── catan-apps-script.js  # Paste into Google Apps Script (backend only, not served)
└── README.md
```

---

## How It Works

### Scoring

Each of the 15 rounds gets its own row. On **mobile**, use the +/− steppers in Lance's or Kat's tab. On **desktop**, type directly into the table cells. Scores sync between the two views automatically.

Bonus rows for **Longest Road**, **Largest Army**, and **Gold Symbols** sit below the 15 rounds. Totals and winner status update live.

### Save Game

When the game is complete, tap **Save Game**. This POSTs to your Apps Script Web App, which appends a row to the Google Sheet:

| Date | Lance | Kat | Winner | Rounds Played |
|------|-------|-----|--------|---------------|
| 5/15/2026 | 42 | 38 | Lance 🏆 | 15 |

### Game History

Tap **Game History** to expand the panel. It fetches all past games from the Sheet (newest first) and shows a win tally at the top — Lance wins, Kat wins, ties.

### Island Map

The **Island Map** panel shows the Island One hex grid with:
- All **15 road segments** numbered in build sequence (starting road marked)
- **6 settlements** (pts 3, 4, 5, 7, 9, 11) shown as house icons at their road-adjacent nodes
- **4 cities** (pts 7, 12, 20, 30) shown as castle icons
- **6 knights** (K1–K6) shown as shields on each terrain hex, with their joker resource labeled

### Rules Reference

The **How to Play** panel covers the full Island One rules: rolling mechanics, all building costs and sequencing rules, knights and Resource Jokers, Gold trading, Longest Road, Largest Army, and end-game scoring.

---

## Building Costs (Island One)

| Build | Cost | Points | Rules |
|-------|------|--------|-------|
| Road | 1 Brick + 1 Lumber | 1 pt | Must connect to existing road; build in sequence |
| Settlement | 1 Lumber + 1 Brick + 1 Grain + 1 Wool | 3→11 pts | Must be adjacent to built road; build lowest pts first |
| City | 3 Ore + 2 Grain | 7→30 pts | Must be adjacent to built road; build lowest pts first |
| Knight | 1 Ore + 1 Grain + 1 Wool | — | Earns a one-time Resource Joker; build in order |

**Bonuses:** Longest Road (all 15 roads) +2 pts · Largest Army (3+ knights, most) +2 pts  
**Penalty:** Fail to build anything in a round → −2 pts (mark X)

---

## Tech Stack

| Layer | Choice | Why |
|-------|--------|-----|
| Frontend | Vanilla HTML/CSS/JS | Zero build step, works offline, deploys as a single file |
| Fonts | Google Fonts (Cinzel, IM Fell English, Crimson Text) | Parchment / board-game aesthetic |
| Backend | Google Apps Script Web App | Free, no account beyond Google, data stays in a Sheet you own |
| Hosting | GitHub Pages | Free static hosting, deploys on push |
| Storage | Google Sheets | Shared between devices, human-readable, no database setup |

---

## Updating After Changes

Any edit to `index.html` or the Apps Script requires a separate step:

**HTML changes:** commit and push to GitHub — Pages redeploys in ~1 minute.

**Apps Script changes:** after editing the script, you must re-deploy:
1. Click **Deploy → Manage deployments**
2. Click the pencil (edit) icon on your existing deployment
3. Change "Version" to **New version**
4. Click **Deploy**

> Saving the script without re-deploying won't update the live endpoint.

---

## Credits

Game design: **Klaus Teuber** · Published by **Catan GmbH**  
Scoresheet design & code: **Lance Terrill** · [lanceterrill.github.io](https://lanceterrill.github.io)
