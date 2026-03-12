# Chrome Web Store Listing Content

## Short description (132 chars max)
Capture DOM events, console logs, network requests, and screenshots. Export structured debug reports for AI coding agents.

## Detailed description

Debug Helper records your browser session and exports everything as a structured report that AI coding assistants can read and act on.

**What it captures:**
- User interactions — clicks, inputs, scrolls, form submissions with element context and timestamps
- Console output — errors, warnings, and logs with stack traces
- Network requests — fetch and XHR with status codes, duration, and response bodies for failures
- Screenshots — capture the visible tab, annotate with rectangles, arrows, text, counters, and crop

**How it works:**
1. Click the extension icon and press Start Recording
2. Browse normally — all interactions are captured automatically
3. Take screenshots and add notes as needed
4. Stop recording and export as Markdown, JSON, or TOON

**Export formats:**
- Markdown — paste directly into AI chat (Claude, ChatGPT, Cursor)
- JSON — programmatic consumption and CI pipelines
- TOON — token-optimized format for LLMs, ~40% fewer tokens than JSON

**Built for developers who work with AI:**
- Auto-generated summary gives the AI the full story in one line
- Relative timestamps (+1.8s) instead of verbose ISO dates
- Smart deduplication collapses redundant input/change/submit events
- Sensitive data (tokens, API keys, passwords) auto-redacted before storage
- ZIP export bundles the report with screenshot files

**Privacy:**
All data stays on your device. No servers, no analytics, no tracking. Sensitive data is automatically redacted at capture time.

No external dependencies. Works in Chrome and Chromium-based browsers (Brave, Edge, Arc).

## Category
Developer Tools

## Language
English

## Privacy policy URL
https://vibery-studio.github.io/debug-helper/privacy.html

## Support URL
https://github.com/vibery-studio/debug-helper/issues

## Permission justifications

### activeTab
Used to capture screenshots of the currently active tab when the user clicks the Screenshot button.

### scripting
Used to inject content scripts that record DOM events (clicks, inputs, scrolls), console output, and network requests during a debug recording session.

### tabs
Used to read the current tab URL and title to include in debug reports as context for the recorded session.

### storage
Used to store recorded session data (events, metadata) locally on the user's device. No data is transmitted externally.

### sidePanel
Used to display a side panel UI showing a live feed of captured events during a recording session.

### alarms
Used to schedule periodic event buffer flushes (every 2 seconds) and storage quota checks to manage local data.

## Store screenshots needed
1. 1280x800 — Popup UI showing recording controls and recent sessions
2. 1280x800 — Side panel with live event feed during recording
3. 1280x800 — Export preview showing Markdown report output
4. 1280x800 — Screenshot annotator with annotation tools
5. 1280x800 — Session history with view/export/delete actions
