# Artífice Connect — Chrome Extension

Browser relay extension for [Artífice](https://artificeia.mx) — AI-driven browser automation for Mexican businesses.

Clients chat on WhatsApp, the AI agent controls their browser on legacy portals (SAT, IMSS, insurance companies, etc.) that lack APIs.

> **Based on** the resilient [OpenClaw Browser Relay](https://github.com/Unayung/openclaw-browser-relay) fork by [@Unayung](https://github.com/Unayung), which is itself based on the Browser Relay from [OpenClaw](https://github.com/openclaw/openclaw) by Peter Steinberger.

## How It Works

1. Install this extension
2. Install the [Artífice Connect](https://github.com/bazfer/artifice-client) desktop app
3. Log into your portal (SAT, insurance, etc.)
4. Click the extension icon on the tab you want to share → badge turns **ON**
5. Chat on WhatsApp — the AI agent can now help you fill forms, submit data, pull information

The agent **never sees your password** — you log in yourself. The agent only operates on already-authenticated sessions.

## Architecture

```
Chrome (your portal) ←CDP→ Extension ←localhost→ Artífice App ←WSS→ Gateway
```

- **This extension** handles Chrome DevTools Protocol (CDP) access to shared tabs
- **Artífice Connect app** handles secure connection to the Artífice gateway
- **Gateway** routes WhatsApp messages to your browser session

## Features (inherited from upstream)

| Feature | Description |
|---------|-------------|
| Auto-reconnect | Exponential backoff with jitter on connection drop |
| State persistence | Survives Chrome service worker restarts (MV3) |
| Tab lifecycle | Proper cleanup when tabs are closed/replaced |
| Keepalive | Alarm-based worker keepalive (every 4 min) |
| Race-safe | WS handlers installed before connect resolves |

## Install

### From Chrome Web Store (coming soon)
Search for "Artífice Connect" in the Chrome Web Store.

### Sideload (development)
1. Clone this repo
2. Open Chrome → `chrome://extensions`
3. Enable **Developer mode**
4. Click **Load unpacked** → select this folder
5. Pin the extension icon

## Configure

1. Click the extension icon → **Options**
2. Set the **Port** (default: `18792`)
3. Enter your **client token** (provided during pairing in the Artífice app)
4. Click **Save**

## Security

- **Tab isolation:** Only tabs you explicitly share are visible to the agent
- **No passwords:** Agent operates post-login only
- **TLS:** All traffic encrypted via WSS
- **Per-client tokens:** Your connection is authenticated and revocable
- **Audit log:** Every agent action is logged

## Development

```bash
git clone https://github.com/bazfer/artifice-connect-extension.git
cd artifice-connect-extension
# Load unpacked in chrome://extensions
```

### Key files
- `manifest.json` — Extension manifest (Manifest V3)
- `background.js` — Service worker: WebSocket client, CDP relay, state management
- `options.html/js` — Configuration page (port, token)

## Related

- [Artífice Connect App](https://github.com/bazfer/artifice-client) — Desktop connection manager (Tauri)
- [Artífice Architecture](https://github.com/bazfer/mind-vault/blob/main/shared/projects/artifice/artifice-client-architecture.md) — Full system design
- [OpenClaw](https://github.com/openclaw/openclaw) — AI agent framework

## License

MIT — see [LICENSE](LICENSE).

## Credits

- Original Browser Relay: [OpenClaw](https://github.com/openclaw/openclaw) by Peter Steinberger
- Resilience patches: [@Unayung](https://github.com/Unayung)
- Artífice adaptation: [@bazfer](https://github.com/bazfer)
