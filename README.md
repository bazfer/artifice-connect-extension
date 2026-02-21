# OpenClaw Browser Relay (Resilient Fork)

A hardened version of the [OpenClaw](https://github.com/openclaw/openclaw) Browser Relay extension that survives WebSocket disconnects, MV3 service worker restarts, and tab lifecycle events.

> **Upstream PR:** [openclaw/openclaw#17588](https://github.com/openclaw/openclaw/pull/17588)
> If/when the PR is merged, this repo may be archived.

## Why This Fork?

The stock Browser Relay extension has several failure modes that require manual re-attachment:

| Problem | Stock | This Fork |
|---------|-------|-----------|
| Gateway restart / WS drop | ❌ Debugger detached, extension goes dead | ✅ Keeps debugger, auto-reconnects |
| MV3 service worker killed | ❌ All state lost, appears attached but dead | ✅ State persisted & restored |
| Tab closed/replaced | ❌ Stale entries left in memory | ✅ Proper cleanup listeners |
| Worker idle timeout (~30s) | ❌ Worker terminated, sessions killed | ✅ Keepalive alarm every 4 min |

## What's Changed

### Fix 1: Don't detach debugger on WS drop
Keep debugger sessions alive when the relay WebSocket closes. Show ⏳ badge while reconnecting. Re-announce sessions when WS recovers.

### Fix 2: Auto-reconnect with exponential backoff
Retries connection with backoff (1s → 2s → 4s → ... → 30s cap) + jitter. On reconnect, verifies each tab's debugger and re-announces to the relay.

### Fix 3: Persist state to `chrome.storage.session`
Saves tab/session state so MV3 service worker restarts can recover without user intervention.

### Fix 4: Tab lifecycle cleanup
Listeners for `chrome.tabs.onRemoved` and `chrome.tabs.onReplaced` to clean up stale entries.

### Fix 5: Keepalive via `chrome.alarms`
A `relay-keepalive` alarm (every 4 min) that keeps the worker alive, pings debugger sessions, and triggers state recovery if needed.

### Fix 6: Race window fix
Permanent WS `onclose`/`onerror` handlers are installed inside `onopen` (before the connect promise resolves) to eliminate the race window where a WS drop could go unnoticed.

## Install (Sideload)

1. Download or clone this repo
2. Open Chrome/Edge → `chrome://extensions` (or `edge://extensions`)
3. Enable **Developer mode**
4. Click **Load unpacked** → select this folder
5. Pin the extension icon in the toolbar

## Configure

1. Click the extension icon → **Options**
2. Set the **Port** (default: `18792`)
3. Enter your **Gateway token** (from `openclaw.json` → `gateway.auth.token`)
4. Click **Save**

## Usage

1. Start OpenClaw Gateway (`openclaw gateway start`)
2. Navigate to any page you want to automate
3. Click the extension toolbar icon — badge turns **ON**
4. OpenClaw can now control that tab via the `browser` tool

## Tested With

- Edge (Chromium) on Linux
- 12+ hours continuous uptime with automated cron jobs (Suno music pipeline)
- Multiple gateway restarts — auto-reconnect worked every time
- Edge Sleeping Tabs — keepalive prevented worker termination

## License

MIT — same as [OpenClaw](https://github.com/openclaw/openclaw/blob/main/LICENSE).

## Credits

Based on the Browser Relay extension from [OpenClaw](https://github.com/openclaw/openclaw) by Peter Steinberger.
Resilience patches by [@Unayung](https://github.com/Unayung).
