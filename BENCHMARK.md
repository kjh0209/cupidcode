# Benchmark: Cupid vs Opus 5.5 in Claude Code (2026-10-22)

We ran the same 16 coding tasks twice in Claude Code, once with Cupid connected and once on Opus, and compared what each side cost and how good the result was. This page explains how, and what the numbers do and don't show.

## Result in short

On 2026-10-22, across the same 16 coding tasks:

- **Cost: 54% lower in total** than Opus ($2.47 charged by Cupid across the tasks vs. $5.39 for Opus).
- **Quality: not worse than Opus in 81% of blind judgments** (88 of 108), from four judges that are not Claude.
- **Hidden final checks: 15 of 15 passed** on the Cupid side (14 of 15 for Opus). One task had no such check.
- **Savings vary by task, and some tasks cost more than Opus.** In this run one task cost 31% more on Cupid.
- **Cupid was slower** on 14 of 16 tasks, often around three times as long.

## Setup

- **Client:** Claude Code (`claude -p`; 2.1.306 on the Opus side, 2.1.308 on the Cupid side), the same flags on both sides, including `--effort high` and the same allowed tools.
- **Cupid side:** Claude Code connected to the Cupid engine at `api.cupidcode.ai` with a test account. Cupid picked the models.
- **Opus side:** Claude Code on Opus 5.5 with a subscription login.
- **One run per task per side.** Each task started from a clean copy of the same starting folder.
- **Follow-up turns:** when a task needed another turn (for example, the model asked a question), a scripted user gave both sides the same fixed reply. No model played the user.
- Models that may train on your input were **off** for the Cupid account during the test.

## The 16 tasks

Twelve tasks come from our own projects (private code), three are synthetic tasks we made for this test, and one uses a public profile page with the owner's consent. We don't publish the private code or the task wording. By kind:

| Kind of task | Tasks |
|---|---|
| Web app: add a feature (build must pass) | 5 |
| Fix a bug from a description of the symptom | 2 |
| Small Node API: add a feature with tests | 1 |
| Add unit tests and a minimal test setup | 1 |
| Git: commit pending changes | 1 |
| Explain how existing code works (no edits) | 1 |
| Diagnose a problem and check a current fact on the web (no edits) | 1 |
| Build a page from scratch from a public web profile | 1 |
| Image input: fix a screen from a screenshot with red notes | 1 |
| Documents: read an Excel sheet and a Word memo, write a CSV | 1 |
| Documents: read a Korean Hangul (`.hwpx`) file, write JSON | 1 |

We planned 17 tasks. One was dropped because our test harness failed on it.

## How cost was measured

- **Cupid:** the amount actually charged to the test account for each task, from the gateway's billing records. The cost of judging (which used the same account) was taken out. Measured as the change in the account balance instead, Cupid's total comes to $2.35, which would make the saving a little larger; we use the higher $2.47.
- **Opus:** the cost Claude Code reported for the session at API rates. A subscription is not billed per call, so this is what the same session would cost on the API with Opus only.
- **Saving** for a task is 1 − Cupid ÷ Opus. The 54% is over the totals: 1 − $2.47 ÷ $5.39.

## How quality was judged

- Each pair of results (Cupid and Opus on the same task) was shown to four judge models from other companies, none of them Claude.
- The judges did not know which side was which. Model names, self-introductions, timing and Cupid's result block were removed first, and every pair was judged twice with the order swapped.
- If Cupid used a model from a judge's own company on a task, that judge's votes for that task were left out.
- "Not worse than Opus" counts wins and ties: 23 wins, 65 ties, 20 losses out of 108 votes.
- Separately, each task that had one ran a **hidden final check**: a test the models never saw, such as opening the page in a browser and checking the button text and color, or comparing the CSV with the right answer.

## Results per task

| Kind of task | Saving | Cupid | Opus | Blind votes (Cupid win / tie / Opus win) | Hidden check (Cupid / Opus) |
|---|---|---|---|---|---|
| Web app feature | 82% | $0.093 | $0.529 | 0 / 8 / 0 | pass / pass |
| Web app feature | 81% | $0.164 | $0.840 | 12 / 0 / 0 | pass / not passed |
| Web app feature | 80% | $0.148 | $0.724 | 0 / 5 / 1 | pass / pass |
| Web app feature | 40% | $0.302 | $0.500 | 0 / 5 / 1 | pass / pass |
| Web app feature | 36% | $0.302 | $0.469 | 0 / 4 / 0 | pass / pass |
| Bug fix from a symptom | 94% | $0.014 | $0.221 | 0 / 6 / 0 | pass / pass |
| Bug fix from a symptom | −31% | $0.128 | $0.098 | 1 / 11 / 0 | pass / pass |
| Node API feature with tests | 79% | $0.030 | $0.143 | 0 / 7 / 1 | pass / pass |
| Unit tests and test setup | 22% | $0.403 | $0.519 | 0 / 0 / 4 | pass / pass |
| Git commit | 20% | $0.050 | $0.062 | 2 / 0 / 4 | pass / pass |
| Explain code | 36% | $0.117 | $0.185 | 1 / 3 / 2 | pass / pass |
| Diagnose + current fact | 10% | $0.247 | $0.273 | 0 / 1 / 5 | no check |
| Page from a public profile | 45% | $0.220 | $0.402 | 6 / 0 / 0 | pass / pass |
| Image input (screenshot) | 61% | $0.091 | $0.235 | 1 / 5 / 0 | pass / pass |
| Excel + Word → CSV | 10% | $0.101 | $0.113 | 0 / 6 / 0 | pass / pass |
| Hangul `.hwpx` → JSON | 24% | $0.059 | $0.077 | 0 / 4 / 2 | pass / pass |
| **All 16** | **54%** | **$2.47** | **$5.39** | **23 / 65 / 20** | **15 / 14 of 15** |

Most tasks got up to eight votes, fewer where a judge was left out. Two tasks were judged on two result pairs and got 12. Amounts are rounded.

## What this does and doesn't show

- **One run per task.** A single task's numbers can move a lot between runs.
- **These are our own tasks.** They come from the kind of work we do, and they aren't a public benchmark. Your work may save more or less.
- **The Opus figure is an API-rate equivalent**, not a bill. If you pay for a subscription, what you actually pay depends on your plan.
- **Judges are models, not people.** We used four of them and swapped the order to reduce bias, but they can still be wrong.
- **Cupid was slower.** Speed is not part of the 54% or the 81%.
- **Cupid changes over time.** Models and routing are updated on the server, so a run next month will not give exactly these numbers. When we run it again, we'll post the new numbers here with their date.

The number shown at the end of each request in the product ("Savings vs Opus") is a different thing: an estimate for that one request, not a side-by-side run. See [How Cupid picks a model](https://cupidcode.ai/docs/routing).
