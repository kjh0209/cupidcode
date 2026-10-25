# Changelog

Release notes for the Cupid CLI and tool connections, newest first, starting with 0.3.0 (the first release on the current engine). Releases before 0.3.0 used an earlier engine and are not listed here.

## 0.3.11 — 2026-10-25

**Opus comparison uses current Opus pricing**

- **Spend comparisons use today's Opus price.** When Cupid estimates what a request would have cost on Opus (for the per-request charge limit and the savings shown to you), it now falls back to current Claude Opus 5.5 pricing instead of an older, retired Opus price.

## 0.3.10 — 2026-10-25

**Clear message for unreadable images, and a private scratch folder per run**

- **A broken image now gets a plain explanation.** If an attached image can't be read (corrupted, unsupported, or too large), Cupid says so in plain words instead of showing a technical error, and removes that image from your restored message so sending again works.
- **Each run uses its own scratch folder.** Temporary files (for example rendered PDF pages) now go to a folder that belongs to the current run and is cleaned up afterwards, so a new session can never pick up files left behind by an earlier one.
- **Claude Code connection: no more environment-scan prompt.** With Cupid connected, Claude Code no longer offers to "teach auto mode about your environment" (a scan of your shell history and other projects). Turning Cupid off restores your previous setting.

## 0.3.9 — 2026-10-25

**Stop really stops, and attachments no longer break a request**

- **Fixed: a session could stay "busy" after you pressed Stop.** If a background task finished after Stop, the session kept showing as working and new messages waited behind it. Stop now ends the turn and the session is ready for your next message.
- **Image attachments respect the size limit.** Images attached as local files now follow the same configured size limit as pasted images: a large image is resized to fit, and one that still cannot fit is refused instead of being sent.
- **A PDF that cannot be rendered no longer ends your request.** If PDF pages cannot be turned into images on your machine (for example when Poppler is not installed), the request continues with the PDF's text, and the model is told plainly which pages it could not see.

## 0.3.8 — 2026-10-24

**Cupid starts reliably on busy machines, and engine checks no longer interrupt you**

- **Fixed: Cupid could stay on "Still starting … · sync" forever.** On a very busy machine the local engine took more than 10 seconds to get ready, and its first requests were lost. Cupid now waits for the engine to be ready before it sends anything.
- **The start screen says what it is waiting for.** If starting takes longer than usual, you see which settings are still loading. If the engine has answered, you can press Enter after 20 seconds to open Cupid while the rest keeps loading. If the engine never answers, Cupid stops after 5 minutes and explains why instead of waiting forever (set `CUPID_STARTUP_TIMEOUT_S` to change this, `0` waits forever). Ctrl+C quits from the start screen.
- **Cupid exits with a non-zero code when it could not start**, so scripts can tell.
- **Cupid's own completion checks run without a permission prompt.** When the Cupid engine checks its work with read-only commands (files exist, `package.json` defines a script, JavaScript syntax), Cupid no longer asks you to approve each one. Only those exact read-only checks inside your project qualify; any other command, a check that touches files outside your project, and Python syntax checks still ask. Your own `deny` rules and command-specific `ask` rules still apply.

## 0.3.7 — 2026-10-24

**See what the Cupid engine is doing, and what you were charged**

- **The sidebar's Spent and the Context "spent" line now show the amount Cupid charged** when you use the default Cupid engine. They use the same gateway total as the end of each request, including subagents. Before, they showed "—" on this path. The VS Code session header and usage panel use the same amount.
- **Removed lines that only repeated "Cupid Engine".** On the default engine path the sidebar no longer shows the Calls list, the "Cupid Engine ×N" line, the call count, or "→ Cupid Engine" under each plan step. VS Code drops the same lines. When real model names are known, they are still shown.
- **Removed the "Actual cost (admin)" row for customer accounts.** It no longer appears as "unavailable".
- **See what the Cupid engine is doing while you wait.** On the default engine path, the folded line above the answer shows the engine's current step (for example "Reading files…" or "Running checks…"), and the sidebar shows the same line while a request runs.
- **"Waited 12.8s" replaces "Thought: 12.8s" on the default engine path.** The time is how long you waited for the engine, not model thinking.
- The Context "spent" line uses the same precision as Spent (four decimals under $0.01).

0.3.6 was not released separately; its changes are included here.

## 0.3.5 — 2026-10-23

**Choose whether Cupid may learn from your requests**

- **New, separate setting: Cupid router learning.** On your first interactive run, Cupid asks once whether it may store your requests (prompts, code context and results) to pick better models. Declining changes nothing else. Change it anytime with `cupid data routing on|off`.
- **`cupid data` now has two settings:** `cupid data routing on|off|status` (router learning) and `cupid data models on|off|status` (whether models that may train on your inputs can be used). `cupid data status` shows both; `cupid data on|off` still means `models`.
- **The end of each request shows your session total**, for example "This session: $X · N% saved vs Opus", using the amounts the Cupid gateway reports (shown only when the gateway provides them).
- **The amount for a request is shown again when you use the default Cupid engine.** It is the total Cupid charged for that request, as recorded by the Cupid gateway, including any subagents. In 0.3.3 it showed "—" on this path.
- The question is never shown in non-interactive runs (`cupid run`, pipes, VS Code).

0.3.4 was not released separately; its changes are included here.

## 0.3.3 — 2026-10-23

**Request amounts always match what you were charged**

- **The amount shown for a request is the sum of what Cupid charged for it**, including any subagents it started and calls charged $0. It is the same number as your usage history.
- **If any call in a request has no reported charge, the amount shows "—"** instead of an estimate. Previously a small uncharged subagent call could be hidden and a slightly different dollar amount shown.
- Cost displays in the CLI and VS Code read only the charge fields the Cupid gateway reports.

## 0.3.2 — 2026-10-23

**English interface text, charged amounts only**

- **All text Cupid itself adds to the CLI and VS Code is in English.** Model replies are unchanged.
- **Costs on screen are the amounts Cupid charged**, as reported by the Cupid gateway. Estimates are no longer shown as charges.

## 0.3.1 — 2026-10-22

**Korean results with images, cleaner summaries, tokens kept out of tool shells**

- **Korean requests with an image attached now get a Korean result block.** The language was being decided by the attachment notes the CLI adds, not by what you wrote.
- **Request summaries show only what you wrote.** Attachment notes and your home folder path no longer appear in summaries or usage history (older rows are masked when read).
- **Changed files are listed by their path in your project**, without the working folder name in front.
- **Usage history shows which tool a request came from** (CLI or VS Code).
- **Your Cupid token is no longer passed to commands the model runs.** Shell tools, MCP servers, formatters, language servers, hooks and template commands no longer receive `CUPID_API_KEY` or any value that starts with `cupid_sk_`. If a script that the model runs called `cupid` and relied on `CUPID_API_KEY` for sign-in, sign in with `cupid auth` instead so the token comes from your keychain.
- **The VS Code sidebar shows your last 30 days again**: requests, amount charged and saving versus Opus, read from the same usage records as the gateway.
- Internal status labels are filtered out of the progress panel in more forms (JSON keys, snake or camel case, dashes).

## 0.3.0 — 2026-10-22

**New engine by default, live progress, connect your other coding tools**

- **`cupid "request"` now runs on the new Cupid engine.** It works out what kind of task you asked for, picks the model with the best chance of success for the cost, runs checks, hands off to a stronger model only when needed, and ends with a short result block: done or verified, anything left to do, the files changed, what you spent and the saving versus Opus (compared with what the request would have cost on Opus alone).
- **See progress as it happens.** The side panel shows each attempt (model and why it was chosen), the checks, any hand-off, the review and the result, plus cost, saving versus Opus and balance. Narrow terminals fold it into one line.
- **Use Cupid from Claude Code, Codex and OpenCode.** `cupid connect` finds the tools you have and sets them up; `cupid connect claude|codex|opencode [on|off]`, `status`, `doctor`, `update` and `remove` (restores your original files). Settings are signed and refreshed from the server, so new models and routing changes need nothing from you.
- **Models that may train on your input are off by default.** `cupid data on|off|status` controls whether those models can be used.
- **One source for numbers.** `cupid stats` reads the same usage records as the gateway, so the CLI, the status line and the web console agree.
- **No session budget cap by default.** Each request is charged at most what it would have cost on Opus alone.
