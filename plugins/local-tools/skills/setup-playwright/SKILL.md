---
name: setup-playwright
description: Use when setting up playwright-cli so Claude can drive a repo's app in a browser locally to reproduce bugs and check changes, including installing playwright-cli and a browser when they're missing.
---

# Set up playwright-cli

Gets playwright-cli working against this repo's app running locally, and records what's needed to use it again.

## Before anything else
The app must already run locally from the terminal. Don't check for, install, or configure playwright-cli until this passes:

1. Read the build, start, stop, and URL steps from CLAUDE.local.md.
2. Start the app with them and confirm the local URL responds, then stop it.

If the steps are missing or the app doesn't come up, stop here. Run setup-local-run, or tell the user it's needed, and continue only once the app starts and responds.

## Approach
1. Check for playwright-cli with `playwright-cli --version`. If it's missing, install it (see Install).
2. Learn the commands from `playwright-cli --help` or the installed playwright-cli skill rather than assuming them.
3. Exclude `.playwright-cli/` from git (see Gotchas).
4. Find the auth method for local runs. Ask how to sign in; never store credentials.
5. Start the app, open one page, and take a screenshot.
6. Record in CLAUDE.local.md: how to start the app, the local URL, the `--browser` value if not the default, how auth works, and the main pages worth checking.

## Install
Ask before installing anything. Needs Node.js 18 or newer.

1. Install the CLI globally, never into the repo:
   ```
   npm install -g @playwright/cli@latest
   ```
2. Get a browser. Prefer one already on the machine, since managed machines often block browser downloads: open with `--browser=chrome` or `--browser=msedge`. Otherwise download one with `playwright-cli install-browser chromium`.
3. Install the agent skill into the home directory:
   ```
   playwright-cli install --skills --global
   ```
4. Verify: `playwright-cli open about:blank`, then `playwright-cli close`.

## Gotchas
- Every run writes snapshots and screenshots to `.playwright-cli/` in the current directory. Add it to `.git/info/exclude` (the shared git dir in a worktree) before the first run.
- `playwright-cli install --skills` without `--global` writes skill files into the repo. Plain `playwright-cli install` initializes a workspace there. Don't run either inside the repo.
- The CLI reads `.playwright/cli.config.json` from the repo. Don't create one; pass `--browser` and other options as flags and record them in CLAUDE.local.md.
- When an update is available, every command prints an update banner first. It's not an error; read past it.

## Guardrails
- Localhost only. Never point it at a shared environment.
- Read-only flows until the app runs against a local database set up by setup-local-db. No submits, saves, or deletes.

## Don't
- Add Playwright config, packages, or tests to the repo.
- Create or edit CLAUDE.md.
- Stage or commit anything. Files this skill creates stay untracked, excluded through `.git/info/exclude`, until the user explicitly asks to commit them.

## Done when
playwright-cli is installed, the app starts, one page opens on localhost, a screenshot shows it loaded, and `git status` lists nothing new.
