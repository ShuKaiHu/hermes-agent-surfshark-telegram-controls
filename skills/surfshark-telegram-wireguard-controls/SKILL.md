---
name: surfshark-telegram-wireguard-controls
description: Add Surfshark manual WireGuard control to Hermes, including tool registration, Telegram /surfshark buttons, status panel rendering, and region-based config discovery on macOS/Homebrew.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [hermes, surfshark, wireguard, telegram, gateway, macos, homebrew]
---

# Surfshark Telegram WireGuard Controls

Use this skill when a Hermes user wants native Surfshark control through Hermes tools and Telegram buttons, backed by local WireGuard `.conf` files.

## What this adds

1. Hermes tools:
   - `surfshark_connect`
   - `surfshark_disconnect`
   - `surfshark_status`
   - `surfshark_change_region`
2. A `surfshark` toolset in `toolsets.py`
3. A gateway-only `/surfshark` slash command
4. Telegram inline buttons for quick region switching
5. A status panel shown when `/surfshark` is opened with no args

## Files involved

- `tools/surfshark_tool.py`
- `model_tools.py`
- `toolsets.py`
- `hermes_cli/commands.py`
- `gateway/run.py`
- `gateway/platforms/telegram.py`
- `tests/tools/test_surfshark_tool.py`
- `tests/gateway/test_surfshark_command.py`
- `tests/gateway/test_telegram_approval_buttons.py`
- `tests/hermes_cli/test_commands.py`

## Requirements

- `wg` and `wg-quick` installed
- Local Surfshark WireGuard `.conf` files already exported from Surfshark manual setup
- On macOS/Homebrew, configs commonly live in `/opt/homebrew/etc/wireguard`
- Config files should be `chmod 600`

## Region discovery pattern

Map friendly region commands to real local config filenames.

Example working mapping:

- Japan
  - command aliases: `connect-japan`, `japan`, `jp`
  - preferred filenames: `jp-tok.conf`, `jp-tokyo.conf`, `japan.conf`, `jp.conf`
- Taiwan
  - command aliases: `connect-taiwan`, `taiwan`, `tw`
  - preferred filenames: `tw-tpe.conf`, `tw-taipei.conf`, `tw-tai.conf`, `taiwan.conf`, `tw.conf`
- Hong Kong
  - command aliases: `connect-hongkong`, `connect-hk`, `hongkong`, `hong-kong`, `hk`
  - preferred filenames: `hk-hkg.conf`, `hk-hongkong.conf`, `hongkong.conf`, `hk.conf`

Implementation pattern in `gateway/run.py`:

- `_surfshark_region_specs()` returns labels, aliases, preferred filenames, stem matches, and prefix matches
- `_surfshark_config_candidates(region_key)` searches likely WireGuard directories and returns candidate files in priority order
- `_resolve_surfshark_region_key(raw)` maps slash-command args to internal region keys

## Telegram UX pattern

### Slash command behavior

When the user runs `/surfshark` with no args:

1. Query current WireGuard status in a background thread
2. Build a short summary like:
   - `現在連線狀態：已連線`
   - `目前節點：香港`
   - `目前 IP：x.x.x.x`
   - `Endpoint：host:51820`
3. Send Telegram inline buttons using `send_surfshark_picker()`

### Inline button layout

Recommended layout:

- `🇯🇵 連到日本`
- `🇹🇼 連到台灣`
- `🇭🇰 連到香港`
- `⛔ 中斷 Surfshark`

Button callback data pattern:

- `ss:connect:japan`
- `ss:connect:taiwan`
- `ss:connect:hongkong`
- `ss:disconnect`

### Callback handling

In `gateway/platforms/telegram.py`:

- route `ss:` callback data inside `_handle_callback_query()`
- implement `_handle_surfshark_callback()`
- convert the callback into a synthetic command event such as:
  - `/surfshark connect-japan`
  - `/surfshark connect-taiwan`
  - `/surfshark connect-hongkong`
  - `/surfshark disconnect`
- call `await self._message_handler(event)` directly so the result can be rendered back into the same message
- keep the inline keyboard attached after completion
- if final edit fails, fall back to `bot.send_message(...)`

## Status panel rendering

Useful helper pattern:

- `_format_surfshark_status_summary(result)` in `gateway/run.py`
- `_render_surfshark_picker_text(status_text=None, status_label="目前狀態")` in `gateway/platforms/telegram.py`

Recommended status output:

- `現在連線狀態：已連線` or `未連線`
- `目前節點：台灣 / 香港 / 日本`
- `目前 IP：...`
- `Endpoint：...`

Use `目前狀態` when opening the panel and `最近結果` after a button action.

## macOS/Homebrew command execution notes

When `wg-quick` comes from Homebrew, prefer explicit Bash 5 invocation:

- `/opt/homebrew/bin/bash /opt/homebrew/bin/wg-quick up <config>`
- `/opt/homebrew/bin/bash /opt/homebrew/bin/wg-quick down <config>`

Do not rely on plain `sudo wg-quick ...` on macOS when Homebrew is involved, because Bash 3 vs Bash 5 resolution can break `wg-quick`.

## Testing checklist

Add or keep these tests:

1. Tool tests
   - region normalization
   - config listing
   - region resolution
   - unknown region rejection
   - connect/disconnect/status/change-region behavior
   - Homebrew bash wrapping
2. Gateway tests
   - `/surfshark` appears in Telegram command registry
   - `/surfshark` with no args opens the picker
   - `/surfshark` no-arg panel includes current status text
   - connect-region success text includes endpoint and IP
   - callback dispatch rewrites to the correct synthetic command
   - callback fallback send works when final edit fails
3. Telegram button tests
   - button labels match intended regions
   - callback data matches the region command wiring

## Safe sharing guidance

Before publishing the skill or upstreaming code:

- remove any personal filesystem paths
- remove account IDs and chat IDs tied to a private environment
- never include real Surfshark public keys, private keys, or full exported `.conf` contents
- use generic example config names only
- usernames/display names are usually fine to keep if the user is intentionally publishing under their own GitHub identity, but secrets and VPN config material are never okay to publish

## Share strategy that worked well

There are two different public-share artifacts here, and keeping them separate is cleaner:

1. **Clean public skill repo**
   - best for sharing reusable workflow knowledge
   - should contain only small public artifacts such as:
     - `README.md`
     - `SKILL.md`
     - `docs/setup.md`
     - `docs/upstream-pr-notes.md`
     - `LICENSE`
   - should NOT contain the full Hermes source tree if the goal is discoverability and clean presentation
   - this worked better than repurposing a whole Hermes fork, which made the GitHub project page look like a generic Hermes codebase instead of a focused Surfshark integration

2. **Upstream code PR / fork**
   - best for contributing the actual Hermes implementation
   - keep the real code changes in a branch or fork against `NousResearch/hermes-agent`

A practical pattern that worked:
- create a minimal repo dedicated to the skill/integration docs
- point README installation instructions at either reading `SKILL.md` directly or adding the repo as a custom tap
- keep upstream implementation notes in `docs/upstream-pr-notes.md`

## Suggested publish strategy

- Share this skill publicly as reusable workflow knowledge
- Prefer a clean companion repo for public skill/docs sharing
- Submit the actual Hermes code changes as a PR so others get the feature natively
