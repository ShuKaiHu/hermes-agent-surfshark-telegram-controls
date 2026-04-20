# Hermes Agent Surfshark Telegram Controls

A clean, shareable repository for a Hermes Agent integration that adds Surfshark WireGuard controls, Telegram inline buttons, and a live status panel.

This repo is intentionally minimal. It is meant to document and share the integration pattern without bundling the full Hermes Agent source tree.

## What is included

- A reusable Hermes skill:
  - `SKILL.md`
- Setup notes:
  - `docs/setup.md`
- Upstreaming notes for contributing the feature back to Hermes Agent:
  - `docs/upstream-pr-notes.md`
- License information

## What this integration does

The integration pattern adds:

- native Surfshark tools for Hermes
- a `surfshark` toolset
- a gateway-only `/surfshark` command
- Telegram buttons for:
  - Japan
  - Taiwan
  - Hong Kong
  - disconnect
- a status summary panel showing:
  - current connection state
  - current region
  - public IP
  - WireGuard endpoint

## Intended environment

This is aimed at Hermes users who:

- use Telegram gateway
- use Surfshark manual WireGuard configs
- want one-tap region switching from Telegram
- run on macOS/Homebrew or another local WireGuard-compatible setup

## Example commands

```text
/surfshark
/surfshark connect-japan
/surfshark connect-taiwan
/surfshark connect-hongkong
/surfshark disconnect
/surfshark status
```

## Privacy model

This repository is designed to be publicly shareable.

It should never contain:

- real Surfshark private keys
- exported personal `.conf` contents
- personal chat IDs
- local machine-identifying paths
- access tokens or API secrets

All examples should stay generic.

## Relationship to Hermes Agent

This repository is not a full Hermes fork.
It is a clean companion project containing the shareable skill and documentation for the Surfshark Telegram control workflow.

Upstream Hermes Agent:
- https://github.com/NousResearch/hermes-agent

## Next steps

If you want this integration to become native for all Hermes users, use the notes in `docs/upstream-pr-notes.md` and open a PR against Hermes Agent.

## License

MIT. See `LICENSE`.
