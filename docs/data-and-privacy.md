# Privacy and telemetry

> Source: cupidcode.ai/docs. If this copy and the website differ, the website is current.

A coding agent has to send source code to a model to do its job. This page describes exactly what goes where, what is off by default, and how to reduce it further.

## What leaves your computer

### 1. Gateway calls (required for the task)

When you send a request, the Cupid gateway (`api.cupidcode.ai`) forwards it to a model provider. What goes out in one turn:

- Your prompt and conversation history
- Contents of files the agent read, and tool results (shell output, search results, diffs)
- Project instructions (`AGENTS.md`, rules, loaded skills)
- Your account token (for authentication)

The gateway holds the provider keys and passes the request to the model that was picked. The same applies when Claude Code, Codex or OpenCode is connected to Cupid.

**Your input is not used to train third-party LLMs.** What the gateway keeps:

| What | How long | Why |
|---|---|---|
| Usage records: time, models used, tokens, cost, the kind of task, the check command and a one-line check result, and the first 60 characters of your latest message (values that look like secrets are masked) | 90 days | So you can see your usage (`cupid stats`, `/cupid`) |
| Working notes for a request in progress (its goals, the last check result) | Up to 6 hours after the last activity | So a request can continue across turns and restarts |
| Billing ledger (charges and top-ups) | Kept as a record | Billing |

The gateway does not store the full text of your prompts, files or model replies, with two exceptions. Accounts that accepted the pilot terms: pilot content is stored encrypted and kept for 5 years from collection. Accounts that agreed to router learning (an optional consent, off unless you agree): requests and results are stored encrypted to improve the Cupid router, kept for up to 5 years from collection and then deleted automatically; if you withdraw consent, the stored raw data is deleted within 30 days, except router weights already trained and anonymous statistics. Neither is used to train third-party LLMs or sold. See the [Privacy Policy](https://cupidcode.ai/privacy-en).

Which providers can receive a request is listed in the [Privacy Policy](https://cupidcode.ai/privacy-en).

List paths that must not be read in `.cupidignore` (`.gitignore` syntax). Put secret files such as `.env` there. The agent will not read or modify those paths.

### Models that may train on your input

Some model providers may use what you send them to train their own models, or have not said that they won't. **Those models are off by default.** Cupid does not send your requests to them unless you turn them on:

```
cupid data status     # see the setting and the list of affected models
cupid data on         # allow them (asks you to confirm in the terminal)
cupid data off        # stop using them from the next request
```

The same setting is `/cupid data on|off|status` in Claude Code and `cupid connect data …` after the one-line install. Turning it on can lower cost. Input already sent to one of those providers can't be taken back. This setting changes which models Cupid may use; it does not change what Cupid itself stores. The setting is not shown in the status line or result block, only when you ask with these commands.


### 2. Device ID and telemetry (off by default)

Telemetry is **off by default.** It is sent only if you set `experimental.openTelemetry: true` in your configuration. If you want a hard guarantee regardless of configuration, set the environment variable below. When enabled, the following is sent:

- An anonymous device ID: a random UUID stored in the `telemetry-id` file in the data directory. It is not derived from hardware information
- App version, OS name, version, and architecture, and surface (CLI or extension)
- Feature usage events (which commands you used). **Prompts, code, and file paths are not included**
- When signed in, your account email is linked to the device ID

How to turn it off (already the default):

```
{ "experimental": { "openTelemetry": false } }
```

The environment variable `CUPID_TELEMETRY_LEVEL` takes precedence over the configuration. Any value other than `all` (for example, `off`) disables sending regardless of the configuration. To change your device ID, delete the `telemetry-id` file; a new one is created on the next run.

### 3. Session sharing (unavailable by default)

Session sharing works only if you configure a **sharing service run by your own organization** (`enterprise.url`). Without it, `/share` is unavailable and no session is uploaded anywhere. If configured, choose `manual` (command only), `auto`, or `disabled` with the `share` setting, and use `/unshare` to take a share down. Transcripts contain code and file paths, so review them before sharing.

### 4. Update checks

On startup, Cupid fetches a version string from `downloads.cupidcode.ai` to check for a new version. No information other than the version string is sent. Turn this off with `autoupdate: false`.

## What stays on your computer

| What | Where | Notes |
|---|---|---|
| Account token | `auth.json` in the data directory | Do not share or commit |
| Sessions, messages, tool results | Local SQLite (`cupid db path`) | Delete with `cupid session delete <id>` |
| File snapshots (for undo) | Data directory | Deleted along with everything else by `cupid uninstall --purge` |
| Logs | `log/` in the data directory | Review contents before attaching to a support request |
| Telemetry ID, profile cache | `telemetry-id`, `telemetry-profile.json` in the data directory | Only relevant if telemetry is enabled |

Check the paths with `cupid debug paths`. To delete everything, run `cupid uninstall --purge`.

## What gets sent with a support request

The support form and the "Report" button on the crash screen send data **only when you click send**. The crash screen pre-fills the error message, stack trace, version, OS, and terminal name; prompts and code are not included. If you attach a session, use `cupid export --sanitize` to create a copy with sensitive data removed.

## Personal information on screen

With `privacy_mode: true`, the TUI blurs your account email, balance, and team name, and profile details are shown only after confirmation. Turn it on when screen sharing or recording.

## Further questions

Contact [support](https://cupidcode.ai/support). The company's overall handling of personal information (retention periods, international transfers, how to exercise your rights) is covered in the [Privacy Policy](https://cupidcode.ai/privacy-en) ([Korean version](https://cupidcode.ai/privacy)). Use of the service is governed by the [Terms of Service](https://cupidcode.ai/terms).

