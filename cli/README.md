# Cupid CLI

The Cupid CLI (`cupid`) is a terminal coding agent. Each request is routed by Cupid to the model with the best chance of success for the cost, and ends with a result block showing the models used, the cost and the saving versus Opus.

- **Install**: [docs/install/cupid-cli.md](../docs/install/cupid-cli.md)
- **Connect Claude Code, Codex or OpenCode**: [docs/install/claude-code-codex-opencode.md](../docs/install/claude-code-codex-opencode.md)
- **Release notes**: [CHANGELOG.md](../CHANGELOG.md)

## Source

This first release publishes the CLI as signed binaries only. The source is planned for a later release. The CLI is licensed under MIT ([LICENSE](LICENSE)); it is derived from opencode and Kilo Code, whose copyright notices are kept in the license file.

## Verify a download

The installers do this for you. To check by hand: download `manifest.json` and `manifest.sig` for the version from `https://downloads.cupidcode.ai/releases/<version>/`, verify the ed25519 signature, then compare the binary's `sha256` with the manifest entry for your platform.
