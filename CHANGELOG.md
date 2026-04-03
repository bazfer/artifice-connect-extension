# Changelog

## 1.0.0 (2026-04-03)

First public release of Artífice Connect.

### Features
- Connect your browser to your AI assistant via secure WSS relay
- Tab-level sharing control (click to share/unshare)
- Auto-reconnect with exponential backoff + jitter
- State persistence via `chrome.storage.session` (survives MV3 worker restarts)
- Keepalive alarm to prevent service worker termination
- Configurable relay URL (default: `wss://relay.artificeia.mx`)
- Token-based authentication

### Based on
Resilient fork of [OpenClaw Browser Relay](https://github.com/Unayung/openclaw-browser-relay) (MIT).
