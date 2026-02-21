# Changelog

## 0.1.0 (2026-02-15)

Initial release — resilient fork of OpenClaw Browser Relay.

### Added
- Auto-reconnect with exponential backoff (1s → 30s cap) + jitter
- State persistence via `chrome.storage.session` (survives MV3 worker restarts)
- Keepalive alarm (4 min interval) to prevent worker termination
- Tab lifecycle cleanup (`onRemoved`, `onReplaced`)
- Per-tab error handling in state restore loop

### Changed
- WS disconnect no longer detaches debugger — keeps sessions alive
- Permanent WS handlers installed inside `onopen` to close race window
- `manifest.json`: added `alarms` permission
