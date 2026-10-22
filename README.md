# work-mode

> Claude Code skills for setting up a repo so the agent can run, seed, and drive the app locally

Each skill walks Claude through one setup step, shows you the plan before acting, and records the result in `CLAUDE.local.md`, which stays out of git. Nothing touches tracked files, shared databases, or credentials.

## Install

```
/plugin marketplace add byteorem/work-mode
/plugin install work-mode@work-mode
```

## Usage

Run the skills in order inside the repo you want to set up:

```
/work-mode:setup-local-claude
/work-mode:setup-local-run
/work-mode:setup-local-db
/work-mode:setup-playwright
```

## Skills

### setup-local-claude

Creates or extends `CLAUDE.local.md` with how to work with the repo on this machine, and excludes it through `.git/info/exclude`.

### setup-local-run

Finds the terminal commands to build, start, and stop the app, verifies them against the local URL, and records them.

### setup-local-db

Builds a local database from the repo's own schema, seeds it with generated mock data, and points the app at it through an untracked override.

### setup-playwright

Installs [playwright-cli](https://github.com/microsoft/playwright-cli) globally if it's missing, then gets it driving the running app on localhost, read-only until a local database exists.
