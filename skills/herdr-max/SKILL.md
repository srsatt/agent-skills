---
name: herdr-max
description: Orchestrate Herdr app and core skills with this setup's repository conventions for work tabs, worktrees, persistent terminals, and agents. Use when spawning agents in Herdr, opening a worktree tab, or troubleshooting the Herdr layout.
---

# Herdr max

Load `herdr-app` for desktop interface, layout, keys and mods; load `herdr-core` for persistent terminals and agents. This skill adds the repository conventions on top of them. This setup has the `hc` and `herdr-app` CLIs (`herdr-app ls`, `herdr-app do <command>`, `herdr-app commands`); `herdr-app` needs the window open. Read `hc --skill` before operating Herdr Core and the app's installed `AGENTS.md` before interface changes.

## Open a work tab from a script or agent

`herdr-work-tab` builds the dashboard's work tab headlessly (Neovim above a shell on the left, agent on the right) in the repository's existing space. Use it instead of a bare `herdr-app do tab.new`, which makes only a single plain terminal.

```bash
herdr-work-tab <worktree-folder> --name "JT-123 merge-prep" --agent claude --task "first prompt"
herdr-work-tab --repo ~/Documents/prog/youtrack --branch my-branch --name "JT-123" --task "..."   # create worktree too
```

Options: `--space NAME|ID`, `--base REF`, `--no-agent`, `--focus`. An existing folder is reused with no setup. A new branch is created at `~/.herdr/worktrees/<repo>/<slug>` from the `repos.json` base, and its `post_create` commands run. The agent starts through `m <profile> [agent]`, with `--task` as its first message. It prints the tab and pane ids as JSON.

The repository space is created when missing. The dashboard repo binding is recorded through the `herdr-bar.bind_work_tab` mod command. Verify with `herdr-app ls` (the tab has three terminals) and `hc get <terminal>` / `hc read <terminal>` (the agent's first message). Close a test tab with `herdr-app do tab.close '{"id":"<tab>","confirmed":true}'`.

## Repository work

- Prefer the repository space and worktree tab provided by the Herdr dashboard. The work tab has Neovim above a shell on the left and the agent on the right. `Cmd+E` opens the dashboard; select the repository, then `w` to open its worktrees. The dashboard opens existing worktrees or creates new ones with this layout.
- When asked to spawn agents using the repository workflow, give each agent a visible Herdr worktree tab in the correct repository space with the standard editor, shell, and agent layout, so it can be found through `Cmd+E`. Create or open the worktree with `herdr-work-tab` (or the dashboard), which starts the requested agent in its agent pane; do not leave it only as a detached `hc` terminal. Confirm the agent and tab are visible in the dashboard. If the interface or required access is unavailable, report that the visible workflow is incomplete rather than claiming success.
- When creating a worktree, inspect the repository's entry in `~/.config/herdr-app/repos.json` and honor its base and ordered `post_create` setup commands. The dashboard executes these on creation; opening an existing worktree does not. Check whether setup already ran before running it manually, and report any failure instead of assuming the worktree is ready. Respect authorization and sandbox boundaries for commands and secrets.
- A bare `hc run` terminal or an ad-hoc tab running `hc attach` is not automatically a repository work tab. If the user wants to see an existing agent in the repository workflow, preserve its running session and place it in the agent pane of a proper worktree tab, with Neovim above a shell on the left and the agent on the right; do not start a duplicate agent. A terminal adopted into the wrong space inherits that space's dashboard project identity. Inspect the desktop layout and use `hc get` before moving or adopting it; verify the splits, worktree, space and `Cmd+E` dashboard identity. If the existing session cannot be placed in that layout, report the limitation instead of treating an ad-hoc attachment as complete. Do not close a tab containing a live agent without verifying what happens to that process.
- Repo bindings and recently opened worktrees determine dashboard project and worktree labels. Use the dashboard's repository/worktree flow so it records those bindings, and check the actual view after changes.

See `~/dotfiles/herdr-app/README.md` for the current layout, shortcuts, and worktree behavior. Do not assume a successful headless command proves the visible layout.
