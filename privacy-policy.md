# Privacy Policy — Artífice Connect

**Last updated:** April 3, 2026

## Overview

Artífice Connect is a browser extension that allows your AI assistant to view and interact with web pages you choose to share. This privacy policy explains what data the extension accesses and how it is handled.

## Data Collection

**Artífice Connect does not collect, store, or transmit any personal data.**

The extension does not:
- Track your browsing history
- Collect analytics or usage data
- Store cookies or user profiles
- Send data to any third party
- Access tabs you have not explicitly shared

## Data Access

When you click the extension icon to share a tab, the extension:
- Attaches the Chrome DevTools Protocol debugger to that specific tab
- Relays page content and interaction commands between your browser and your configured Artífice relay server
- Stops all access when you click the icon again to unshare

## Communication

All communication between the extension and your relay server is:
- Encrypted using WSS (WebSocket Secure) over TLS
- Authenticated with a client token you configure
- Direct — no intermediate servers, no data storage in transit

## Permissions

- **Debugger:** Used to read page content and perform actions (clicking, typing, navigating) on tabs you explicitly share. The debugger is only attached to tabs you activate — never to tabs you haven't shared.
- **Host permissions (relay.artificeia.mx):** Used to establish a secure WebSocket connection to your Artífice relay server.

## Data Retention

Artífice Connect stores only your configuration (relay URL and client token) in local browser storage. No browsing data, page content, or interaction history is retained.

## Open Source

The full source code is available at: https://github.com/bazfer/artifice-connect-extension

## Contact

For questions about this privacy policy, contact: privacy@artificeia.mx

## Changes

We may update this privacy policy from time to time. Changes will be reflected in the "Last updated" date above.
