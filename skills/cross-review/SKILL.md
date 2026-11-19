---
name: cross-review
description: Review changes with a different agent harness, such as Codex reviewing Claude's work or the reverse, as that harness's own reviewer role in a fresh context. Use when a review needs a different model family than the author, or when the user asks for a cross-model or second-opinion review.
---

# Cross review

An author's model family shares the author's blind spots, and so does its
context. This skill starts the other harness as its configured `reviewer`
role, which inspects the changes with read-only tools and knows nothing about
how they were made.

## Requirements

- The other harness installed and signed in: `codex`, or Claude Code.
- A `reviewer` role in that harness: `~/.claude/agents/reviewer.md` for Claude,
  `$CODEX_HOME/reviewer.config.toml` for Codex (`codex exec -p reviewer`).
- The command `cross-review` from this skill's `scripts/` directory on `PATH`.

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
  the prompt. Claude gets read, search, and `git status|diff|show|log` only;
  Codex runs in its read-only sandbox.
- A nested call exits with an error, so a reviewer cannot hand the review back.
- Claude runs with no MCP servers and at most 30 turns
  (`CROSS_REVIEW_MAX_TURNS`). When you run the command from a tool with its own
  timeout, set that to at least 10 minutes.
- To pick an account, set the harness's own variable for the command, for
  example `CODEX_HOME=... cross-review codex origin/main`.
- `CROSS_REVIEW_DRY=1` prints the prompt and exits without calling the other
  harness.

## Use the result

1. Check each finding against the code before acting on it. The reviewer works
   from the changes and can be wrong.
2. Report confirmed findings with `file:line`, the failure scenario, and a fix.
3. If the command fails, say so and review in the current harness instead. Do
   not retry in a loop.
