# Xcelevate Skills Foundation — Live Event Dashboard

A real-time, single-page event dashboard that displays live pledge data, supporter names, photos, and metrics for Xcelevate Skills Foundation events.

## Quick Start

### GitHub Pages (Recommended)

1. Upload `index.html` to a GitHub repo
2. Go to **Settings → Pages → Source → Deploy from branch → `main` → `/ (root)` → Save**
3. Your site is live at `https://yourusername.github.io/repo-name/`

### Any Static Host

Upload `index.html` to Netlify, Vercel, or any static hosting. It's a single self-contained file — no build step, no dependencies, no other files needed.

### Local Testing

Just open `index.html` in Chrome. It will immediately start fetching live data.

---

## Features

- **Live metrics** — Total pledge amount, supporter count, average pledge, latest pledge with odometer animation
- **Name wall** — Glossy green-black panel showing every supporter with glow-in entrance animation
- **Photo collage** — Horizontal strip that grows as new supporters submit photos (Google Drive links auto-converted to thumbnails)
- **Scrolling ticker** — Continuous horizontal scroll of recent supporter names
- **QR code** — Scannable QR linked to the pledge form, with animated border
- **Auto-polling** — Fetches new data from Google Apps Script every 5 seconds
- **Display/Projector mode** — Add `?display=true` to URL for auto-scrolling kiosk mode optimized for 1080p projectors
- **Stale data detection** — Shows warning if API is unreachable for 30+ seconds
- **Offline resilience** — Exponential backoff on failures, graceful recovery

---

## Display Mode (Projector)

For event-day projection on a screen:

```
https://yourusername.github.io/repo-name/?display=true
```

Then press **F11** for fullscreen. The page will auto-scroll through all sections on a loop with larger fonts optimized for 1080p viewing distance, hidden cursor, and screen wake lock.

---

## Configuration

All configuration is embedded in the `<script>` tag at the top of `index.html`. Key settings:

| Setting | Description |
|---------|-------------|
| `API_URL` | Google Apps Script web app endpoint |
| `API_KEY` | Sent as `?key=` query parameter |
| `POLL_INTERVAL_MS` | Fetch frequency (default: 5000ms) |
| `PHOTO_ROTATE_MS` | Photo strip update interval (default: 3000ms) |

### API Response Format

```json
{
  "ok": true,
  "count": 1,
  "items": [
    {
      "Timestamp": "2026-02-25T13:55:02.731Z",
      "Full Name": "AKASH RATHORE",
      "Pledge Commitment amount": "₹ 20,00,000",
      "Photo URL (uploaded via dashboard)": "https://drive.google.com/file/d/.../view",
      "Email id": "...",
      "Mobile Number": ...
    }
  ]
}
```

**Privacy:** Email and Mobile Number are never displayed.

---

## Troubleshooting

### Data not showing
- Open browser console (F12) and check for errors
- Verify your Apps Script is deployed with **"Anyone"** access
- Test the API URL directly in browser — should return JSON

### CORS errors
Your Apps Script `doGet` must return:
```javascript
return ContentService.createTextOutput(JSON.stringify(data))
  .setMimeType(ContentService.MimeType.JSON);
```

### Photos not loading
Ensure Google Drive files are shared as **"Anyone with the link"** and URL format is `https://drive.google.com/file/d/FILE_ID/view`

---

## Tech Stack

Single HTML file. No build tools, no frameworks, no npm. Uses GSAP 3.12 + ScrollTrigger from CDN, Google Fonts (Playfair Display + DM Sans), and vanilla JS.

---

Built for Xcelevate Skills Foundation.
