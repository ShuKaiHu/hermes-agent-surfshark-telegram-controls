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

## Install this skill

There are three practical ways to use this repository.

### Option 1: Read the skill directly from this repo

Open these files and follow the documented integration steps:

- `SKILL.md`
- `docs/setup.md`
- `docs/upstream-pr-notes.md`

This is the best option if you want to copy the approach into your own Hermes fork or PR.

### Option 2: Add this repo as a custom skill source (recommended for Hermes users)

This repository now includes the skill under:

- `skills/surfshark-telegram-wireguard-controls/SKILL.md`

Recommended real-world install flow:

```bash
hermes skills tap add ShuKaiHu/hermes-agent-surfshark-telegram-controls
hermes skills search surfshark --source github
hermes skills inspect ShuKaiHu/hermes-agent-surfshark-telegram-controls/surfshark-telegram-wireguard-controls
hermes skills install ShuKaiHu/hermes-agent-surfshark-telegram-controls/surfshark-telegram-wireguard-controls --force
```

Why `--force` may be required:

- Hermes may classify this as a community skill with a `CAUTION` verdict.
- The warning is expected because the integration can require privileged WireGuard commands such as `sudo wg-quick` on the host system.
- Review the skill with `inspect` first, then install with `--force` only if you understand and trust the workflow.

If you prefer browsing first, these commands can also help:

```bash
hermes skills browse
hermes skills search surfshark
```

### Option 3: Use the skill as a published source repo

If you are publishing or mirroring skills from a local skill directory, this repository can also be used as the GitHub target:

```bash
hermes skills publish /path/to/surfshark-telegram-wireguard-controls --to github --repo ShuKaiHu/hermes-agent-surfshark-telegram-controls
```

### Notes

- Tap/install UX can vary slightly depending on Hermes version.
- If your Hermes build supports direct GitHub-backed skill discovery, this repo is structured to work as a clean public source.
- If you mainly want to learn the integration and reproduce it yourself, reading `SKILL.md` directly is still the simplest path.

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
