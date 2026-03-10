# Debug Helper

Chrome extension that captures browser debug context (DOM events, console logs, network requests, screenshots) and exports structured reports for coding agents.

![Chrome Extension](https://img.shields.io/badge/Manifest-V3-blue) ![No Dependencies](https://img.shields.io/badge/Dependencies-None-green)

## What it does

Record a browsing session — every click, input, scroll, console error, and network request gets captured with timestamps. Take annotated screenshots along the way. Add manual notes. Export everything as a structured report that AI coding assistants can read and act on.

## Screenshots

| Popup | Live Feed | Session History |
|:---:|:---:|:---:|
| <img src="images/popup-ui.jpg" width="250"> | <img src="images/live-feed.jpg" width="250"> | <img src="images/session-history.jpg" width="250"> |

| Export Preview | Screenshot Annotator |
|:---:|:---:|
| <img src="images/export-preview.jpg" width="350"> | <img src="images/screenshot-annotator.jpg" width="350"> |

## Install

### Option 1: Download release (recommended)

1. Download the [latest release](https://github.com/vibery-studio/debug-helper/releases/tag/latest) ZIP
2. Unzip the file
3. Open `chrome://extensions`
4. Enable **Developer mode**
5. Click **Load unpacked** → select the unzipped folder
6. Pin the extension for quick access

### Option 2: Clone repo

1. Clone this repo
2. Open `chrome://extensions`
3. Enable **Developer mode**
4. Click **Load unpacked** → select the repo folder
5. Pin the extension for quick access

## Usage

**Start recording:** Click the extension icon → **Start Recording** (or `Cmd+Shift+R` / `Ctrl+Shift+R`)

**During recording:**
- Browse normally — all interactions are captured automatically
- Take screenshots: `Cmd+Shift+S` / `Ctrl+Shift+S`
- Add notes: type in the note bar that appears in the popup or side panel
- Open the side panel for live feed of captured events

**Export:** Stop recording → choose format → copy or download ZIP

## Export Formats

| Format | Best for |
|--------|----------|
| **Markdown** | Pasting into chat with AI assistants |
| **JSON** | Programmatic consumption, CI pipelines |
| **TOON** | Token-efficient format optimized for LLMs |

All formats include: step timeline, console errors, network failures, screenshot references, and auto-generated summary.

## Features

- **DOM event capture** — clicks, inputs, scrolls, form submissions with element context
- **Console capture** — errors, warnings, logs with stack traces
- **Network capture** — fetch & XHR with status, duration, response bodies for errors
- **Screenshots** — capture + annotate with rectangles, arrows, text, freehand, counters, crop
- **Manual notes** — add text annotations to the step timeline during recording
- **Auto-redaction** — strips Bearer tokens, API keys, passwords before storage
- **Deduplication** — collapses rapid duplicate clicks and input/change/submit overlaps
- **Storage management** — chunked event storage, auto-cleanup at 80% quota
- **ZIP export** — report + screenshot files bundled together

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Cmd/Ctrl+Shift+R` | Toggle recording |
| `Cmd/Ctrl+Shift+S` | Capture screenshot |

## Architecture

```
Content Scripts (page) → Bridge → Service Worker → Storage → UI
     ↓                                ↓
  DOM events                    IndexedDB (screenshots)
  Console logs                  chrome.storage (events, sessions)
  Network requests
```

**Content scripts** run in two worlds:
- ISOLATED: `recorder.js` (DOM events), `bridge.js` (message relay)
- MAIN: `console-capture.js`, `network-capture.js` (intercept native APIs)

**Service worker** buffers events (flushes every 2s or 50 events), manages sessions, handles exports.

**No external dependencies.** ZIP builder, TOON encoder, and all utilities are built-in.

## File Structure

```
├── manifest.json              # Extension config (MV3)
├── background/
│   └── service-worker.js      # Session, event, export management
├── content/
│   ├── recorder.js            # DOM event capture (ISOLATED)
│   ├── bridge.js              # MAIN→ISOLATED relay
│   ├── console-capture.js     # Console interception (MAIN)
│   └── network-capture.js     # Network interception (MAIN)
├── popup/
│   ├── popup.html/js/css      # Extension popup UI
├── sidepanel/
│   ├── sidepanel.html/js/css  # Side panel with live feed, history, export
├── annotator/
│   ├── annotator.html/js/css  # Screenshot annotation tool
├── devtools/
│   ├── devtools.html/js       # DevTools integration
│   └── panel.html/js
├── lib/
│   ├── storage.js             # Chrome storage + IndexedDB wrapper
│   ├── export.js              # Report generation (JSON/MD/TOON)
│   ├── toon.js                # TOON format encoder
│   ├── zip.js                 # Minimal ZIP builder
│   └── utils.js               # Shared utilities
├── styles/
│   └── common.css             # Global theme and components
└── icons/
    └── icon{16,48,128}.png
```

## Export Example

```markdown
# Debug Report
**URL:** https://example.com
**Duration:** 12400ms

> Clicked "Login". Entered "user@test.com". Submitted form. Note: "Check validation error".

## Steps
1. `+0.3s` Clicked "Login"
2. `+1.2s` Typed "user@test.com" in input#email
3. `+2.8s` Submitted form
4. `+3.1s` 📝 Check validation error — see [Screenshot 1](#screenshot-1)

## Console Errors
- **[ERROR]** `+3.2s`: TypeError: Cannot read property 'value' of null

## Network Errors
- **POST /api/login** → 422 (180ms)
```

## License

GPL-3.0-or-later
