# Cross review initial setup

Read this file only after a `cross-review` run failed. The skill assumes the
tools are already set up, so a failure usually means one of them is not.

## Run

From this skill's directory:

```sh
scripts/setup
```

It is safe to re-run. `BIN_DIR` overrides where the command is linked
(default `~/.local/bin`). It checks that `codex` and `claude` are installed,
links `cross-review` onto `PATH`, reports the optional reviewer roles, and runs
a dry run.

## What to do with the result

Suggest the fix to the user and let them decide:

- `codex not installed` or `claude not installed`: ask the user to install and
  sign in to that harness. Do not install it silently.
- `... is not on PATH`: tell the user to add the directory to their shell
  profile. Do not edit the profile yourself.
- `no ... reviewer role`: optional. Reviews work without a role; a role only
  tunes the reviewer's model and instructions.
- `dry run failed`: run it inside a git repository.
- Sign-in is not verified by the script. If the failure was an authentication
  error, ask the user to run `codex login` or start `claude` once.

After the checks pass, run the review again once.
