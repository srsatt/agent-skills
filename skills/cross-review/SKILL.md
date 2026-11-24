---
name: cross-review
description: Review changes with a different agent harness, such as Codex reviewing Claude's work or the reverse, as that harness's own reviewer role in a fresh context. Use when a review needs a different model family than the author, or when the user asks for a cross-model or second-opinion review.
---

# Cross review

An author's model family shares the author's blind spots, and so does its
context. This skill starts the other harness in a fresh session to inspect the
changes. It uses a configured `reviewer` role when available.

## Requirements

- The other harness installed and signed in: `codex`, or Claude Code.
- The command `cross-review` from this skill's `scripts/` directory on `PATH`.
- Optional: a `reviewer` role at `.claude/agents/reviewer.md` or
  `${CLAUDE_CONFIG_DIR:-~/.claude}/agents/reviewer.md` for Claude, or
  `${CODEX_HOME:-~/.codex}/reviewer.config.toml` for Codex.

## Run

Pass the requirement on stdin. Use `codex` when you are Claude, and `claude`
when you are Codex. Choose what to review with one argument:

```sh
printf '%s' "<requirement the change must satisfy>" | cross-review codex origin/main
printf '%s' "<requirement>" | cross-review codex --uncommitted
printf '%s' "<requirement>" | cross-review codex --commit <sha>
```

- Send only the requirement and the target. Do not send your summary, plan, or
  reasons why the change is correct: they bias the reviewer.
- The reviewer reads the changes itself with git, so no diff is embedded in
  the prompt. Claude is offered read and search tools and preapproved Git read
  commands; existing harness settings can grant additional permissions. Codex
  runs in its read-only sandbox. Use a separate sandbox with least-privilege
  credentials for untrusted changes.
- A nested call exits with an error, so a reviewer cannot hand the review back.
- Claude runs with no MCP servers and at most 30 turns
  (`CROSS_REVIEW_MAX_TURNS`). When you run the command from a tool with its own
  timeout, set that to at least 10 minutes.
- To pick an account, set the harness's own variable for the command, for
  example `CODEX_HOME=... cross-review codex origin/main`.
- `CROSS_REVIEW_DRY=1` prints the prompt and exits without calling the other
  harness.

## CI

Install the chosen harness and configure its non-interactive authentication using
your CI secret store. Check out the target repository with enough Git history
to include the base and merge base. Install this skill or check out this repo
at a pinned revision, then invoke the script by path; adding it to PATH is
optional. For example, from the repository under review:

```sh
printf '%s' "$(cat review-requirement.txt)" | /path/to/skills/cross-review/scripts/cross-review claude origin/main > review.txt
```

The command returns the harness's exit status. A successful run means the
review completed, not that it found no issues: inspect `review.txt` or publish
it as a CI artifact. Do not expose secrets to untrusted pull requests. The
review sends repository content to the selected model provider; follow your
team's data policy. Model and account configuration belong to the harness.

## Use the result

1. Check each finding against the code before acting on it. The reviewer works
   from the changes and can be wrong.
2. Report confirmed findings with `file:line`, the failure scenario, and a fix.
3. If the command fails, say so and review in the current harness instead. Do
   not retry in a loop.
