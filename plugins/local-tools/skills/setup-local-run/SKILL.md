---
name: setup-local-run
description: Use when setting up a repo so Claude can build, start, and stop the app locally on its own. Run this before setup-playwright.
---

# Set up local run

Gets the app building and running locally from the terminal, so the agent can start and stop it without an IDE.

## Approach
1. Check CLAUDE.local.md for existing run steps. If they exist, verify them instead of starting over. If CLAUDE.local.md doesn't exist, run setup-local-claude first.
2. Find how the app builds and starts: solution and project files, manifests, scripts, launch settings, README. State what you found.
3. If the app only starts from an IDE, find the command-line equivalent: the build tool, plus the server the IDE launches. Call tools the way CLAUDE.local.md records them. If a tool isn't recorded, is missing, or is the wrong version, run setup-tools. Ask before installing anything.
4. Find what the app needs at startup: config, environment variables, a database, other services. Note which environment each one points at.
5. Show the plan before running anything.
6. Build, start the app in the background, and wait until the local URL responds.
7. Stop the app and confirm the port is free.
8. Record in CLAUDE.local.md: build, start, and stop commands, the local URL and port, where logs go, and startup dependencies.

## Guardrails
- Don't change tracked config, launch settings, or project files to make it run. If that seems required, stop and ask.
- If startup depends on a shared database or service, say so. Don't write to it.
- Never record credentials.
- Don't create or edit CLAUDE.md.
- Don't stage or commit anything. Files this skill creates stay untracked, excluded through `.git/info/exclude`, until the user explicitly asks to commit them.

## Done when
The agent builds the app, starts it, gets a response from the local URL, and stops it cleanly, all from the terminal.
