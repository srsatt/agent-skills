---
name: cross-review
description: Review the current branch with a different agent harness, such as Codex reviewing Claude's work or the reverse, in a fresh context over the Agent Client Protocol. Use when a review needs a different model family than the author, or when the user asks for a cross-model or second-opinion review.
---

# Cross review

An author's model family shares the author's blind spots, and so does its
context. This skill sends the diff to the other harness, which reviews it
read-only with no knowledge of how the change was made.

## Requirements

- `acpx` on `PATH` (`npm install -g acpx`), the headless ACP client.
- The other harness installed and signed in: `codex`, or Claude Code.
- The command `cross-review` from this skill's `scripts/` directory on `PATH`.

## Run

Pass the requirement on stdin and the base ref as an argument. Use `codex`
when you are Claude, and `claude` when you are Codex.

```sh
printf '%s' "<requirement the change must satisfy>" | cross-review codex origin/main
```

- Send only the requirement and the base ref. Do not send your summary, plan,
  or reasons why the change is correct: they bias the reviewer.
- The command embeds `git diff <base>...HEAD` in the prompt. It asks the
  other harness for read and search access only, through an ACP permission
  policy and, for Claude, an allowed-tools list. This is best effort: the
  harness's own rules and auto mode can still allow more, so run it in a
  checkout you can discard if the repository is untrusted.
- A nested call exits with an error, so a reviewer cannot hand the review back.
- The wait is limited to 540 seconds; set `CROSS_REVIEW_TIMEOUT` to change it.
  When you run the command from a tool with its own timeout, set that above
  540 seconds.
- Claude as reviewer needs the user's Claude settings for authentication when
  they route through a proxy: set `ACPX_CLAUDE_INCLUDE_USER_SETTINGS=1`.
- To pick an account or profile, set the harness's own variable for the
  command, for example `CODEX_HOME=... cross-review codex origin/main` or
  `CLAUDE_CONFIG_DIR=... cross-review claude origin/main`.
- `CROSS_REVIEW_DRY=1` prints the prompt and exits without calling the other
  harness.

## Use the result

1. Check each finding against the code before acting on it. The reviewer works
   from a diff and can be wrong.
2. Report confirmed findings with `file:line`, the failure scenario, and a fix.
3. If the command fails, say so and review in the current harness instead. Do
   not retry in a loop.
