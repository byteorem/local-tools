---
name: setup-local-db
description: Use when setting up a local database for an app that defines its own schema, so it can run against local data (generated or copied from an existing database) instead of a shared environment database. Run this after setup-local-run.
---

# Set up a local database

Creates a local database from the schema the repo defines, fills it with generated mock data or data copied from an existing database, and points the app at it.

## Before anything else
The app must already run locally from the terminal, since the last step verifies against it. Read the build, start, stop, and URL steps from CLAUDE.local.md. If they're missing, run setup-local-run first.

## Check that this applies
Only continue if both of these hold:
- **The app uses a database.** It connects to one at startup or when serving requests. Startup dependencies in CLAUDE.local.md are the first place to look.
- **The repo defines the schema.** It has migrations, a database project, or ORM models that generate the schema.

If the app has no database, stop and say so. If it uses a database whose schema is defined somewhere else, ask the user where. If it's another repo they can share, build from that. If it isn't, stop: a schema reverse-engineered from a shared database is guesswork. State what you found either way.

If CLAUDE.local.md already records a local database, verify it rebuilds and the app reads from it instead of starting over.

## Approach
1. Find how the app connects: which config file or setting holds the connection, and how it varies by environment.
2. Pick the local engine that matches what the app uses in its environments, and the repo's own deploy method for the schema (publish, migrate). Don't invent a new one. If the engine or deploy tool isn't installed, run setup-tools rather than installing it.
3. Show the plan before running anything.
4. Create the local database and deploy the schema to it, then seed it (see Data).
5. Point the app at it through a local-only override (see Config).
6. Record in CLAUDE.local.md how to rebuild and reseed it.

## Data
Ask the user where the data comes from before the plan in step 3. There are two options:

**Generated mock data**
- Use a mock data library the repo already depends on. If it has none, use the stack's standard one (Bogus for .NET, Faker for JavaScript or Python) in the seed script only, without adding it to the app's dependencies.
- Keep the seed script outside the schema project and out of tracked files, so the real pipeline never runs it.
- Seed enough for the main workflows to show something, not full coverage.

**Copied from an existing database**
- The user names the source database and signs in to it. Never handle or record its credentials.
- Only read from the source, using the engine's own export or backup tool. Ask before copying a whole database rather than the tables the main workflows need.
- If the source is production or holds personal or customer data, say so and get explicit confirmation before copying. Copying that data to a local machine may break the employer's data policy.
- Keep the export file out of the repo, or exclude its path through `.git/info/exclude`.

## Config
- Prefer an override the app already supports that isn't tracked: user secrets, env vars, a local settings file.
- If the app only reads tracked config, stop and ask before changing anything.
- Never commit a local connection string.

## Guardrails
- Don't write to a shared database. The only thing to run against one is the read-only export the user chose under Data.
- Don't change the schema project or tracked config.
- Don't create or edit CLAUDE.md.
- Don't stage or commit anything. Files this skill creates stay untracked, excluded through `.git/info/exclude`, until the user explicitly asks to commit them.

## Done when
The local database rebuilds and reseeds from the steps in CLAUDE.local.md, and the app starts and loads a page that reads data from it.
