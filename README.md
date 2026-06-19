# Ortus Club · HubSpot Sync — distribution host

This repository exists only to **host the signed Chrome extension** for
force-install on Ortus's managed (Google Workspace) Chrome devices. It serves
two files via GitHub Pages:

- **`ortus-ext.crx`** — the signed extension build.
- **`updates.xml`** — the Chrome update manifest the Admin policy points at.

Extension ID: `fpkljmkadlmoblfafkohhgdcahjcdhfd`

Pages URLs:
- Update manifest: `https://ortusclub.github.io/ortus-hs-ext/updates.xml`
- Download: `https://ortusclub.github.io/ortus-hs-ext/ortus-ext.crx`

## Security

The `.crx` contains **no HubSpot token** — the extension talks to the
`ortus-hs-proxy` Cloudflare Worker, which holds the token server-side. The only
credential in the bundle is a low-value proxy key that is gated to four HubSpot
contact endpoints and is rotatable without a rebuild.

## Releasing an update

From the extension project (not this repo):

1. Bump the version in `manifest.json`.
2. `sh pack-crx.sh` — rebuilds and signs `dist/ortus-hubspot-sync-<ver>.crx`
   with `ortus-ext-signing-key.pem` (kept private, never committed anywhere).
3. `sh distribution/gen-updates.sh "https://ortusclub.github.io/ortus-hs-ext"`.
4. Copy the new `.crx` here as `ortus-ext.crx`, copy the new `updates.xml`, and
   push. Managed Chrome picks up the higher version automatically.
