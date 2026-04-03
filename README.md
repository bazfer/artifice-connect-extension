# Artífice Connect

Chrome extension that connects your browser to your AI assistant, enabling your agent to view, navigate, and interact with web pages — with your permission and oversight.

## Features

- **Tab-level control** — Click the toolbar icon to share/unshare individual tabs
- **Secure connection** — WSS/TLS encrypted, token-authenticated
- **Auto-reconnect** — Resilient WebSocket connection with exponential backoff
- **State persistence** — Survives service worker restarts (MV3)
- **No data collection** — Zero tracking, zero analytics

## Setup

1. Install from the Chrome Web Store (or load unpacked for development)
2. Click the extension icon → configure your relay URL and client token
3. Navigate to any page and click the extension icon to share the tab

## Development

```bash
git clone https://github.com/bazfer/artifice-connect-extension.git
cd artifice-connect-extension
# Load unpacked in chrome://extensions with Developer Mode enabled
```

## Privacy

See [Privacy Policy](privacy-policy.md).

## License

MIT
