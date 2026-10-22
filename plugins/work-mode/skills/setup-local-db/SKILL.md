---
name: setup-local-db
description: Use when setting up a local database for a repo so the app can run without a shared environment database.
---

# Set up a local database

Gives the app a local database built from the repo's own schema and filled with mock data.

## Before anything else
The app must already run locally from the terminal, since the last step verifies against it. Read the build, start, stop, and URL steps from CLAUDE.local.md. If they're missing, run setup-local-run first.

## Approach
1. Find where the schema lives: a database project, migrations, an ORM model, or a separate repo. If it's in another repo, ask where that repo is.
2. Find how the app connects: which config file or setting holds the connection, and how it varies by environment. State what you found.
3. Pick the local engine that matches what the app uses in its environments, and the repo's own deploy method for the schema (publish, migrate). Don't invent a new one. If the engine or deploy tool isn't installed, run setup-tools rather than installing it.
4. Show the plan before running anything.
5. Build the database locally, then seed it (see Data).
6. Point the app at it through a local-only override (see Config).
7. Record how to rebuild and reseed in CLAUDE.local.md.
8. Verify: the app starts and a page that reads data loads from the local database.

## Data
- Generate mock data. Never copy data from a shared or production database.
- Keep the seed script outside the schema project and out of tracked files, so the real pipeline never runs it.
- Seed enough for the main workflows to show something, not full coverage.

## Config
- Prefer an override the app already supports that isn't tracked: user secrets, env vars, a local settings file.
- If the app only reads tracked config, stop and ask before changing anything.
- Never commit a local connection string.

## Don't
- Run anything against a shared database.
- Change the schema project or tracked config.
- Create or edit CLAUDE.md.
- Stage or commit anything. Files this skill creates stay untracked, excluded through `.git/info/exclude`, until the user explicitly asks to commit them.
