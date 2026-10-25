<p align="center">
  <img src=".github/assets/logo.png" width="96" height="96" alt="Cupid logo">
</p>

<h1 align="center">Cupid</h1>

<p align="center"><strong>Same AI coding tool, about half the cost.</strong></p>

<p align="center">
  <a href="LICENSE"><img alt="License: Apache-2.0" src="https://img.shields.io/badge/license-Apache--2.0-blue.svg"></a>
  <a href="CHANGELOG.md"><img alt="Release 0.3.11" src="https://img.shields.io/badge/release-0.3.11-8b3dff.svg"></a>
  <a href="docs/install/"><img alt="Install: one line" src="https://img.shields.io/badge/install-curl%20%7C%20sh-2ea44f.svg"></a>
  <a href="https://cupidcode.ai/docs/benchmark"><img alt="Benchmark: 54% lower cost vs Opus" src="https://img.shields.io/badge/benchmark-54%25%20lower%20cost%20vs%20Opus-ff7a59.svg"></a>
</p>

<p align="center">
  <a href="https://cupidcode.ai">Website</a> ·
  <a href="https://cupidcode.ai/docs">Docs</a> ·
  <a href="https://cupidcode.ai/docs/benchmark">Benchmark</a> ·
  <a href="CHANGELOG.md">Changelog</a>
</p>

Cupid is a model routing engine for coding. Keep the tool you already use (Claude Code, Codex, OpenCode) or use the Cupid CLI. Cupid picks the right model for each request, checks the work, and brings in a stronger model only when it is needed.

```sh
curl -fsSL https://cupidcode.ai/install.sh | sh
```

Then create a token at [dev.cupidcode.ai/tokens](https://dev.cupidcode.ai/tokens). Full instructions: [docs/install](docs/install/).

## How it works

1. **Route.** Each request is classified by the kind of task, and sent to the model with the best chance of success for the cost.
2. **Verify.** Cupid runs checks on the result before calling it done.
3. **Hand off.** If the checks fail or the task is beyond the first model, it hands off to a stronger model. Every request ends with a short result block: what was done, anything left to do, the files changed, the cost, and the saving versus Opus.

## Measured result

Paired test on 2026-10-22: the same **16 coding tasks** run in Claude Code twice, once connected to Cupid and once on Opus 5.5.

| | Result |
|---|---|
| **Billed cost** | **54% lower** than Opus: $2.47 charged by Cupid vs. $5.39 for Opus (API rates) |
| **Quality** | **Not worse than Opus in 81%** of blind judgments (88 of 108), from four judges that are not Claude |
| **Hidden final checks** | **15 of 15** passed on the Cupid side (14 of 15 for Opus); one task had no such check |

Savings vary by task: one task cost 31% more on Cupid, and Cupid was slower on 14 of 16 tasks. One run per task, on our own tasks, not a public benchmark. Method, per-task results and limits: [cupidcode.ai/docs/benchmark](https://cupidcode.ai/docs/benchmark) and [BENCHMARK.md](BENCHMARK.md).

**Providers:** routes across models from Anthropic, OpenAI, Google, DeepSeek, Mistral, xAI, Moonshot AI, Alibaba Cloud, Z.ai, Cohere and more. The full list of recipients is in the [Privacy Policy](https://cupidcode.ai/privacy-en).

## What's in this repository

This is Cupid's public home for releases, documentation, issues and the benchmark. The CLI ships as signed binaries; its source is not in this repository yet. Routing, model choice and pricing run on Cupid's servers and are not open source.

| Path | What it is |
|---|---|
| [docs/install/](docs/install/) | Installing and connecting the four clients: Cupid CLI, Claude Code, Codex, OpenCode (plus VS Code status) |
| [docs/data-and-privacy.md](docs/data-and-privacy.md) | What leaves your computer, what is kept and for how long, and how to turn things off |
| [BENCHMARK.md](BENCHMARK.md) | The 2026-10-22 paired test: method, results for all 16 tasks, limits |
| [cli/](cli/) | Cupid CLI: install, verify, license |
| [vscode/](vscode/) | VS Code extension: status, license |
| [CHANGELOG.md](CHANGELOG.md) | Release notes |

Binaries are downloaded from `downloads.cupidcode.ai`. The installer checks an ed25519-signed manifest and each file's sha256 before installing.

## Data

Models from providers that may train on your input are off by default (`cupid data status` to check). What leaves your computer, what the gateway keeps and for how long: [docs/data-and-privacy.md](docs/data-and-privacy.md).

## Issues and security

- Bugs and feature requests: open an issue. Please don't paste tokens, private code or customer data.
- Security vulnerabilities: do not open a public issue. See [SECURITY.md](SECURITY.md) (security@cupidcode.ai).
- We don't accept pull requests in this repository for now. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Apache-2.0 by default ([LICENSE](LICENSE)). The Cupid CLI and the VS Code extension are MIT ([cli/LICENSE](cli/LICENSE), [vscode/LICENSE](vscode/LICENSE)); part of the VS Code extension is Apache-2.0 ([vscode/THIRD_PARTY_LICENSES/continue.txt](vscode/THIRD_PARTY_LICENSES/continue.txt)). See [NOTICE](NOTICE) and [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).

## Acknowledgements

The Cupid CLI and VS Code extension build on the open-source projects opencode, Kilo Code and Continue. Their copyright notices are kept in the license files above.
