---
name: setup-playwright
description: Use when setting up playwright-cli so Claude can drive a repo's app locally to reproduce bugs and check changes.
---

# Set up playwright-cli

Gets playwright-cli working against this repo's app running locally, and records what's needed to use it again.

## Approach
1. Check that playwright-cli and its browsers are installed. If not, stop and tell the user; don't install anything without asking.
2. Learn the commands from `playwright-cli --help` rather than assuming them.
3. Read the start, stop, and URL steps from CLAUDE.local.md. If they're missing, run setup-local-run first.
4. Find the auth method for local runs. Ask how to sign in; never store credentials.
5. Start the app, open one page, and take a screenshot.
6. Record in CLAUDE.local.md: how to start the app, the local URL, how auth works, and the main pages worth checking.

## Guardrails
- Localhost only. Never point it at a shared environment.
- Read-only flows until the app runs against a local database. No submits, saves, or deletes.
- Keep screenshots and other output out of tracked files.

## Don't
- Add Playwright config, packages, or tests to the repo.
- Create or edit CLAUDE.md.

## Done when
The app starts, one page opens on localhost, and a screenshot shows it loaded.
