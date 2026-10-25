# VS Code extension

> Source: cupidcode.ai/docs. If this copy and the website differ, the website is current.

The VS Code extension is in progress and is not yet available on the Marketplace. When it ships, we will note it on this page and email everyone who signed up for [launch notifications](https://cupidcode.ai/#pricing).

## Current status

- **Marketplace: not yet released (coming soon)**. There is currently no public package to install.
- The terminal TUI (`cupid`) is fully functional. When the extension ships, it will use the same engine, the same account, and the same session data.

## How it works (at launch)

The extension does not have its own engine. It starts your installed `cupid` binary with `cupid serve` and connects to it. This means:

- You need to install the CLI and sign in first: [Install](https://cupidcode.ai/docs/install), [Sign in](https://cupidcode.ai/docs/login).
- Model choice, permissions and the result block are the same as in the TUI. The extension's chat panel shows the models used and the saving versus Opus.
- You can continue a session started in the TUI from the extension, and vice versa. Sessions share a single local database.

## Using Cupid in VS Code today

Run `cupid` in the VS Code integrated terminal. It uses the same working directory as the files open in your editor, and changes show up in the editor immediately. Use `/diff` to view changes inside the TUI.

## Other surfaces

- **Web console**: [dev.cupidcode.ai](https://dev.cupidcode.ai). View run history, costs, and routing rationale.
- **`cupid serve` / `cupid attach <url>`**: run a server on one machine and connect to it from another terminal. Useful for remote development environments.
- **ACP**: `cupid acp` starts an Agent Client Protocol server. Editors that support ACP can use cupid as an agent.

