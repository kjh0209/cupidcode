# Install and update

> Source: cupidcode.ai/docs. If this copy and the website differ, the website is current.

One line installs the Cupid CLI and connects the coding tools you already have. No administrator rights needed, and no provider API keys.

## Install with one line (macOS, Linux)

```
curl -fsSL https://cupidcode.ai/install.sh | sh
```

This does three things:

1. Installs the Cupid CLI (0.3.0 or later) if it isn't installed yet. If it is, the CLI is left as it is.
2. Asks for your Cupid token and stores it in the system keychain (macOS Keychain, `secret-tool` on Linux, or a `0600` file if neither is there). Create a token at [dev.cupidcode.ai/tokens](https://dev.cupidcode.ai/tokens). The token is not shown on screen and is not written into any tool's config file.
3. Finds Claude Code, Codex and OpenCode on your machine and connects the ones you pick to Cupid. Details are in [Claude Code, Codex and OpenCode](https://cupidcode.ai/docs/connect).

It also offers to install small converters so models can read Excel, Word, PowerPoint and Hangul (`.hwp`, `.hwpx`) files. Say no if you don't need them; the install continues either way.

The script checks the sha256 of every file it downloads against signed server settings, never uses `sudo`, and only writes under your home folder. Its own checksum is published at [cupidcode.ai/install.sh.sha256](https://cupidcode.ai/install.sh.sha256).

When it finishes, open a new terminal and check:

```
cupid --version
cupid doctor
cupid connect status
```

> Already have an older CLI (0.2.x)? The one-line install connects your tools but does not upgrade the CLI. Run `cupid upgrade` to move to 0.3.0, which uses the new engine by default.

## Windows

The PowerShell installer installs the CLI only:

```
irm https://downloads.cupidcode.ai/install.ps1 | iex
```

`cupid connect` (the part that connects Claude Code, Codex and OpenCode) does not run on Windows yet, and neither do `cupid auth login` and `cupid data`. On Windows, set your token in the `CUPID_API_KEY` environment variable ([Sign in](https://cupidcode.ai/docs/login)). Models that may train on your input stay off.

## CLI only (macOS, Linux)

If you only want the CLI and not the tool connections:

```
curl -fsSL https://downloads.cupidcode.ai/install.sh | sh
```

The CLI then needs a token: run the one-line install later, or set `CUPID_API_KEY` ([Sign in](https://cupidcode.ai/docs/login)). The other way round also works: `curl -fsSL https://cupidcode.ai/install.sh | sh -s -- --no-cli` connects your tools without installing the CLI.

### Supported platforms

| Platform | Notes |
|---|---|
| `darwin-arm64`, `darwin-x64` | macOS (Apple Silicon, Intel) |
| `linux-x64`, `linux-arm64` | glibc distributions |
| `linux-x64-musl`, `linux-arm64-musl` | Alpine and similar. Requires `libstdc++`: `apk add --no-cache libstdc++` |
| `win32-x64`, `win32-arm64` | Windows 10 or later, PowerShell 5.1+. CLI only |

### What the CLI installer does

1. Detects your OS and architecture.
2. Fetches the current version from `https://downloads.cupidcode.ai/releases/stable` (set `CUPID_CHANNEL=latest` for the other channel).
3. Downloads that version's `manifest.json` and `manifest.sig` and checks the ed25519 signature. If no verifier (node or openssl 3) is available, it warns and falls back to sha256 over TLS.
4. Downloads the binary and compares it with the sha256 in the manifest. On a mismatch it stops and leaves nothing behind.
5. Puts the binary in `~/.cupid/bin` and adds it to PATH in your shell rc file. To leave PATH alone, run the downloaded binary with `cupid install --no-modify-path`.

## Update

```
cupid upgrade            # latest stable
cupid upgrade --check    # only check whether a new version is out
cupid connect update     # refresh the tool connections if the server settings changed
```

`update` is an alias for `upgrade`. To go back to an older version, add `--allow-downgrade`. If an upgrade fails, run the installer again; it installs over the old copy.

New models and routing changes happen on the server, so you don't need to update anything for those. Tool connection settings refresh themselves once a day (`cupid connect autoupdate off` turns that off).

## Uninstall

```
cupid connect remove       # undo the Claude Code, Codex and OpenCode connections
cupid uninstall            # remove the CLI binary and runtime files (keeps config and sessions)
cupid uninstall --purge    # also remove config, sessions and snapshots
cupid uninstall --dry-run  # only print what would be removed
```

`cupid connect remove` puts back the config files exactly as they were before install, unless you edited them since. In that case it takes out only what Cupid added and keeps a backup.

## Troubleshooting

- **`cupid: command not found`**: open a new terminal or run `source ~/.zshrc` (or `~/.bashrc`). If that doesn't help, add `export PATH="$HOME/.cupid/bin:$PATH"` to your rc file.
- **Codex or OpenCode doesn't use Cupid after install**: open a new terminal. They read the token when the shell starts.
- **"Unidentified developer" on macOS**: binaries from the installer don't get this. If you downloaded the binary in a browser, run `xattr -d com.apple.quarantine ~/.cupid/bin/cupid`.
- **Corporate proxy**: set `HTTPS_PROXY` and run again. The installers and `cupid` all respect it.
- **Checksum mismatch**: usually an interrupted download. Run it again. If it keeps happening, tell [Support](https://cupidcode.ai/support) your platform and version.
- **Something looks off**: `cupid connect doctor` checks the server settings, your token and balance, each tool's config and the converters, without spending credit.

