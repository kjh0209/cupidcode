# Claude Code, Codex and OpenCode

> Source: cupidcode.ai/docs. If this copy and the website differ, the website is current.

`cupid connect` points the coding tools you already use at the Cupid engine. You keep working in the same tool; Cupid picks the model behind it and bills your Cupid credits.

## Connect

The [one-line install](https://cupidcode.ai/docs/install) runs this for you. To run it again later:

```
cupid connect                    # finds your tools and asks which to connect
cupid connect claude             # just one tool (claude, codex or opencode)
```

You don't need a Claude subscription or an OpenAI account for this. The tools use your Cupid token and credits. `cupid connect` runs on macOS and Linux; it doesn't run on Windows yet.

After connecting:

| Tool | How to use it | Usage and savings |
|---|---|---|
| Claude Code | Run `claude` as usual | `/cupid` inside Claude Code, plus a status line |
| Codex | Run `codex` in a new terminal | Ask Codex "show my Cupid usage", or run `bash ~/.cupid/cupid-usage.sh` |
| OpenCode | Run `opencode` in a new terminal | `/cupid` and `/cupid-today` |

Each request ends with the same short result block as the CLI: done or verified, anything left to do, the files changed, the cost and the saving versus Opus. See [How Cupid picks a model](https://cupidcode.ai/docs/routing).

## Turn it on and off

```
cupid connect off                # all tools back to their original settings
cupid connect on                 # all tools back to Cupid
cupid connect codex off          # one tool only
cupid connect status             # what is connected right now
```

Turning a tool off puts its settings back the way they were. If you sign in to a Claude subscription, that login is not touched, so `cupid connect claude off` takes you straight back to it.

## Keep it up to date

Model choices, routing and pricing live on the server, so they change without anything on your side. The small files `cupid connect` installs (the `/cupid` command, the status line, the usage script) are refreshed once a day, and only if their signature from our server checks out. If a tool's config file needs a change, you get a one-line notice and run:

```
cupid connect update
```

Turn the daily refresh off with `cupid connect autoupdate off`.

## Check and repair

```
cupid connect doctor             # server settings, token, balance, each tool, converters (no credit used)
cupid connect token              # replace your token
```

## Remove

```
cupid connect remove             # every tool
cupid connect remove opencode    # one tool
```

Files you didn't edit after connecting go back byte for byte. If you edited one, only the lines Cupid added are taken out and a backup is kept in `~/.cupid/connect/backups`.

## What it changes on your machine

- **Claude Code**: adds Cupid's address and model names to `~/.claude/settings.json`, a small helper that reads the token from where the installer stored it (the token itself is not written to the file), the `/cupid` command and a status line.
- **Codex**: adds a `cupid` provider block to `~/.codex/config.toml`, marked so it can be taken out cleanly. Any of your own top-level keys that clash are commented out and restored when you turn it off.
- **OpenCode**: adds a `cupid` provider to `~/.config/opencode/opencode.json` and sets `cupid/cupid-engine` as the model.
- **Shell start file**: one line that reads the stored token for Codex and OpenCode. The token value is not stored there.
- **Converters (if you said yes)**: `~/.cupid/venv` and `~/.cupid/bin/cupid-read`, which turn Excel, Word, PowerPoint and Hangul files into text a model can read. The tools are told to use it.

## Differences between the tools

- Images and PDFs you paste or attach are passed through. Cupid then picks a model that can read them.
- OpenCode asks before running check commands by default. In non-interactive runs, set the permission so it doesn't wait for an answer.
- Thinking output that some tools show for Anthropic models is not shown.

## Models that may train on your input

Off by default. To check or change it:

```
cupid connect data status
cupid connect data on
cupid connect data off
```

`cupid data …` in the CLI and `/cupid data …` in Claude Code do the same thing. See [Privacy](https://cupidcode.ai/docs/privacy#models-that-may-train-on-your-input).

