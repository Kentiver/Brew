# 🍺 Brew Control

A premium homebrewing fermentation dashboard built for the **RAPT ecosystem**. Designed to run full-screen on a dedicated iPad, Brew Control connects directly to the RAPT API and guides you through the entire fermentation process — from pitching yeast to transferring to keg — with automatic temperature control, smart process automation, and a clean premium interface.

---

## What It Does

Brew Control is a single HTML file you host on GitHub Pages. It connects to your RAPT account via API and gives you a real-time fermentation dashboard without needing any apps, servers, or subscriptions beyond your existing RAPT setup.

### Live Data
Every 60 seconds the dashboard fetches fresh data from your RAPT Pill hydrometer:
- Specific gravity (SG) with trend
- Fermentation temperature (from inside the liquid)
- Gravity velocity (how fast fermentation is progressing)
- Battery level — only shown as a warning if below 20%

If no data has been received for 60 minutes, a warning strip appears at the top of the screen.

### Fermentation Progress
A dedicated card shows estimated fermentation progress using the formula:

```
(OG − Current SG) / (OG − Expected FG) × 100
```

This gives you a percentage of how far along fermentation is, along with a progress bar, current SG, expected FG, starting OG, and how much SG remains until the target. A circular progress indicator in the metric grid mirrors the same value with a live-filling arc and a label that updates automatically: **Active fermentation → Halfway → Almost done → Fermentation complete**.

### Temperature Controller
When a RAPT Temperature Controller is connected, the dashboard automatically detects it at login — no manual configuration needed. It displays the actual cabinet temperature and the current setpoint, and allows manual temperature adjustment via a **Juster** button directly on the temperature card.

**Command Queue with verification** — every temperature command goes through a verified 3-step process:
1. Send the command to the controller
2. Wait 5 seconds
3. Read the controller back — is the setpoint confirmed?

- Confirmed → toast notification shown: *"Controller bekreftet 20°C ✓"*
- Not confirmed → retry up to 3 times
- All 3 attempts fail → warning: *"⚠ Temperaturkommando mislyktes"*

All commands and confirmations are logged with timestamps, visible in Advanced mode.

### Batch Types

**Beer (Øl)**
- Automatic diacetyl rest: temperature raised (fermentation temp + 3°C) when SG approaches target FG
- Dry hop alert: triggered when velocity has been below 0.1 for 12 consecutive hours
- Cold crash: after FG stable for 24 hours, or manually after dry hopping
- Transfer to keg: after 48 hours of cold crash

**Cider (Cider)**
- Flavoring alert: triggered when FG stable for 48 hours and SG is below 1.006
- 48-hour countdown after flavoring confirmed, then automatic cold crash
- Transfer to keg: after 48 hours of cold crash

Both types ignore velocity and FG alerts during the first 48 hours after batch start.

### Automated Temperature Control
When a RAPT Temperature Controller is enabled:
- **Batch start** → sets controller to your chosen fermentation temperature
- **Diacetyl rest** → raises temperature automatically (fermentation temp + 3°C)
- **Cold crash** → lowers temperature to 2°C automatically
- **Manual override** → set any temperature at any time; badge shown when active; **↩ Auto** button returns to automatic control

### Safety
- Hard limit of **35 PSI** — any pressure command above this is silently blocked
- All temperature commands are verified by reading the controller back after 5 seconds
- Manual override is always available regardless of automation phase

### Process Checklist
A sidebar on the right side of the dashboard tracks progress through the brew. Steps irrelevant to your batch type (e.g. dry hop when not chosen) are hidden automatically.

| Step | Trigger |
|------|---------|
| ✅ Fermentation started | When batch is created |
| ✅ Diacetyl rest | Automatic — SG near target FG |
| ✅ Dry hops added | When you confirm |
| ✅ Fermentation complete | Automatic — FG stable |
| ✅ Cold crash started | When you press Start Cold Crash |
| ✅ Ready for transfer | Automatic — after 48h cold crash |

### Simple and Advanced Mode
**Simple mode** — clean and minimal: temperature, SG, fermentation progress, controller status, active banners.

**Advanced mode** — adds: 24-hour SG and temperature chart, full command log with timestamps.

### Batch History
When you start a new batch, the previous batch is automatically saved to history, showing batch name, type, start date, OG, final SG, actual ABV achieved, and total duration.

---

## Setup

### What You Need
- A [RAPT Pill hydrometer](https://www.kegland.com.au) registered to your RAPT account
- A RAPT account at [rapt.io](https://rapt.io)
- An **API Secret** — create one under **My Account → Api Secrets** on rapt.io

### Hosting on GitHub Pages (free)
1. Create a free account at [github.com](https://github.com)
2. Create a new **public** repository
3. Upload the file renamed as `index.html`
4. Go to **Settings → Pages → Branch: main → Save**
5. Dashboard is live at `https://yourusername.github.io/repositoryname`

### Using on iPad (full screen)
1. Open the URL in **Safari** on your iPad
2. Tap the Share button → **Add to Home Screen**
3. Open from the home screen icon for full-screen mode

> Always open from the home screen icon, not from Safari directly. The two use separate local storage.

---

## Compatible Hardware

| Device | Support |
|--------|---------|
| RAPT Pill Hydrometer | ✅ Full — gravity, temperature, velocity, battery |
| RAPT Temperature Controller | ✅ Full — read, set, verify, command log |
| RAPT Digital Regulator & Spunding Valve | 🔜 Ready to integrate when available |

---

## Privacy

All login data, batch information, timers and history are stored locally in your browser — nothing is sent to any server other than the RAPT API directly. Since the GitHub repository is public, anyone with the URL can open the dashboard, but they would need your RAPT email and API Secret to log in.

---

## Technical Notes

- Single self-contained HTML file — no build tools, no dependencies, no server required
- Polls RAPT API every **60 seconds**
- Authentication token refreshed every 50 minutes
- All state persisted in browser localStorage
- Command queue retries temperature commands up to 3 times with 5-second verification
- Handles both 1.047 and 1047 gravity formats from RAPT API
- 48-hour warmup period prevents false alerts after pitching

---

*Built for homebrewers using the KegLand / RAPT ecosystem.*
