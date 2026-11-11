---
name: herdr-usage
description: Use and troubleshoot Herdr's desktop layout, persistent terminals, agents, and worktrees using this setup's repository conventions.
---

# Herdr usage

For desktop interface guidance see [herdr-app](/Users/pavel.reutov/dotfiles/codex/skills/herdr-app/SKILL.md); for persistent terminals and agents follow [herdr-core](/Users/pavel.reutov/dotfiles/codex/skills/herdr-core/SKILL.md). This setup has the `hc` CLI but no `herdr-app` CLI: use the desktop interface for window, space, tab, and pane operations, and do not run commands suggested by the app guide that require `herdr-app`. Read `hc --skill` before operating Herdr Core and the app's installed `AGENTS.md` before interface changes.

## Repository work

- Prefer the repository space and worktree tab provided by the Herdr dashboard. The work tab has Neovim above a shell on the left and the agent on the right. `Cmd+E` opens the dashboard; select the repository, then `w` to open its worktrees. The dashboard opens existing worktrees or creates new ones with this layout.
- When asked to spawn agents using the repository workflow, give each agent a visible Herdr worktree tab in the correct repository space with the standard editor, shell, and agent layout, so it can be found through `Cmd+E`. Create or open the worktree through the dashboard, then start the requested agent in its agent pane; do not leave it only as a detached `hc` terminal. Confirm the agent and tab are visible in the dashboard. If the interface or required access is unavailable, report that the visible workflow is incomplete rather than claiming success.
- When creating a worktree, inspect the repository's entry in `~/.config/herdr-app/repos.json` and honor its base and ordered `post_create` setup commands. The dashboard executes these on creation; opening an existing worktree does not. Check whether setup already ran before running it manually, and report any failure instead of assuming the worktree is ready. Respect authorization and sandbox boundaries for commands and secrets.
- A bare `hc run` terminal or an ad-hoc tab running `hc attach` is not automatically a repository work tab. If the user wants to see an existing agent in the repository workflow, preserve its running session and place it in the agent pane of a proper worktree tab, with Neovim above a shell on the left and the agent on the right; do not start a duplicate agent. A terminal adopted into the wrong space inherits that space's dashboard project identity. Inspect the desktop layout and use `hc get` before moving or adopting it; verify the splits, worktree, space and `Cmd+E` dashboard identity. If the existing session cannot be placed in that layout, report the limitation instead of treating an ad-hoc attachment as complete. Do not close a tab containing a live agent without verifying what happens to that process.
- Repo bindings and recently opened worktrees determine dashboard project and worktree labels. Use the dashboard's repository/worktree flow so it records those bindings, and check the actual view after changes.

See `~/dotfiles/herdr-app/README.md` for the current layout, shortcuts, and worktree behavior. Do not assume a successful headless command proves the visible layout.
