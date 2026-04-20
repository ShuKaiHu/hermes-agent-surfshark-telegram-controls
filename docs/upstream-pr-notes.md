# Upstream PR Notes

This repository is a clean companion project for the Hermes Surfshark Telegram integration.

If you want to upstream the actual functionality into Hermes Agent, the main implementation areas are:

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

## Suggested PR summary

- add native Surfshark WireGuard tools and toolset registration
- add a gateway-only `/surfshark` command with Telegram inline controls
- show current Surfshark connection status when opening the control panel
- support Japan, Taiwan, and Hong Kong config discovery with macOS/Homebrew-friendly defaults

## Before opening the PR

- remove all user-specific paths
- remove any local machine identifiers
- remove personal config names that are not generic examples
- never include real private keys or exported `.conf` contents
- ensure tests still pass

## Suggested tests

```bash
pytest tests/tools/test_surfshark_tool.py tests/gateway/test_surfshark_command.py tests/hermes_cli/test_commands.py -q -o 'addopts='
```
