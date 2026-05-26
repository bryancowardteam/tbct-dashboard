# TBCT Command Center

Live dashboard for The Bryan Coward Team. Displays active transactions, upcoming events, and marketing requests.

**Live site:** https://bryancowardteam.github.io/tbct-dashboard/

---

## How It Works

The dashboard is a single-page app (`index.html`) that loads data from `data.json` on page open. The page itself requires no server -- it is hosted via GitHub Pages directly from the `main` branch.

The `data.json` file is refreshed automatically by a Claude scheduled task (`tbct-dashboard-refresh`) that runs Monday, Wednesday, and Friday at 7 AM. That task pulls current transaction, event, and marketing data from connected sources (Trello, Google Calendar, Slack) and pushes an updated `data.json` to this repo via the GitHub MCP.

If `data.json` fails to load (e.g., a network issue), the page falls back to a copy of the data embedded directly inside `index.html`. A stale-data warning banner appears on the dashboard automatically if the last update timestamp is more than 24 hours old.

---

## Files

| File | Purpose |
|------|---------|
| `index.html` | Full dashboard UI and embedded fallback data |
| `data.json` | Live data file fetched on page load |
| `BryanCoward_Logo_Ver_White.png` | Logo used in the dashboard header |
| `tbct-logo.png` | Used as the browser tab favicon |

---

## data.json Schema

```json
{
  "lastUpdated": "2026-05-26T08:00:00",
  "transactions": [
    {
      "address": "123 Main St",
      "city": "Tampa, FL 33601",
      "type": "Listing",
      "status": "Active",
      "price": 500000,
      "mls": "TB1234567",
      "details": "3 Bed / 2 Ba | Single Family Residence",
      "clients": "Jane and John Doe",
      "phone": "(813) 555-0000",
      "email": "janedoe@email.com",
      "closeDate": "2026-07-01",
      "expiry": "2026-12-31",
      "lender": "Lender Name",
      "title": "Title Company Name",
      "trelloUrl": "https://trello.com/c/cardId/card-name"
    }
  ],
  "events": [
    {
      "title": "Event Name",
      "start": "2026-06-01",
      "time": "10:00 AM - 11:00 AM",
      "location": "Zoom"
    }
  ],
  "marketingRequests": [
    {
      "title": "Request Title",
      "status": "in-progress",
      "statusLabel": "In Progress",
      "requestedBy": "Bryan",
      "date": "May 26, 2026",
      "notes": "Notes about this request."
    }
  ]
}
```

**Transaction status values:** `Active`, `Pending`, `Closed`, `Cancelled`

**Transaction type values:** `Listing`, `Buyer`

**Marketing status values:** `in-progress`, `review`, `complete`

---

## Automatic Refresh (Scheduled Task)

The `tbct-dashboard-refresh` Claude scheduled task runs Mon/Wed/Fri at 7 AM and handles the full refresh cycle automatically. EJ does not need to do anything on those days unless the stale-data banner appears on the live site.

If the banner appears, check:
1. Whether the scheduled task ran (visible in Claude Cowork task history)
2. Whether the GitHub MCP was authenticated at the time of the run
3. Whether there was an error in the task log

---

## Manual Update Steps

To update `data.json` manually without waiting for the scheduled task:

1. Open a Claude Cowork chat
2. Say: "Update the dashboard data.json with the latest transaction info and push it to GitHub"
3. Claude will pull from Trello, Google Calendar, and Slack, build the updated JSON, and push it via the GitHub MCP

To edit `data.json` directly:
1. Open the file in this repo on GitHub
2. Click the pencil (edit) icon
3. Make your changes following the schema above
4. Commit directly to `main`

After any manual commit, the live site at the GitHub Pages URL will reflect the change within a few seconds.

---

## Adding or Removing a Transaction

To add a new transaction, ask Claude: "Add [address] to the dashboard as a [Listing/Buyer] in [status] status."

To remove a transaction (e.g., after closing), ask Claude: "Remove [address] from the dashboard data.json."

Closed transactions are intentionally not displayed on the dashboard to reduce clutter.

---

## Team

| Name | Role |
|------|------|
| Bryan Coward | Owner, The Bryan Coward Team |
| EJ Gaba | Executive Assistant -- maintains this dashboard |
| Sarah Bunag | Marketing Director |
| Kimberly Fickett | Transaction Coordinator |
