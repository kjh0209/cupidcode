# Security

## Reporting a vulnerability

Email **security@cupidcode.ai**. Please do not open a public issue, discussion or pull request for a security problem.

Include what you found, how to reproduce it, the affected version (`cupid --version`, `cupid connect status`), and your platform. Do not include real tokens, private code or other people's data; if a token was exposed, revoke it at [dev.cupidcode.ai/tokens](https://dev.cupidcode.ai/tokens) first.

We aim to acknowledge reports within a few business days and will keep you updated until the issue is resolved. Please give us a reasonable time to fix the issue before disclosing it.

## Scope

- The Cupid CLI and installers (`cupidcode.ai/install.sh`, `downloads.cupidcode.ai`)
- `cupid connect` and the files it installs for Claude Code, Codex and OpenCode
- The Cupid gateway (`api.cupidcode.ai`) and console (`dev.cupidcode.ai`)

Out of scope: denial-of-service and volume testing, social engineering, and testing against accounts you don't own.

## Verifying downloads

The installers check an ed25519-signed `manifest.json` and each binary's sha256 before installing, and stop on a mismatch. The one-line installer's own checksum is published at `https://cupidcode.ai/install.sh.sha256`.
