# local-tools

> Claude Code skills for setting up a repo so the agent can run, seed, drive, and scan the app locally

Everything these skills set up is local to your machine. Each skill walks Claude through one setup step, shows you the plan before acting, and records the result in `CLAUDE.local.md` or a local project skill. Every file they create stays untracked through `.git/info/exclude` and is never committed unless you ask. Nothing touches tracked files, shared databases, or credentials.

## Install

```
/plugin marketplace add byteorem/local-tools
/plugin install local-tools@local-tools
```

## Usage

Run the skills in order inside the repo you want to set up:

```
/local-tools:setup-local-claude
/local-tools:setup-tools
/local-tools:setup-local-run
/local-tools:setup-local-db
/local-tools:setup-playwright
/local-tools:setup-security-scan
```

## Skills

### setup-local-claude

Creates or extends `CLAUDE.local.md` and local `.claude/` config (`settings.local.json`, project skills, agents) with how to work with the repo on this machine, and keeps them untracked.

### setup-tools

Checks that the build tools, SDKs, runtimes, package managers, and CLIs the repo needs are installed and match its version pins, finds any that are installed but off PATH, and records how to call them. Installs nothing.

### setup-local-run

Finds the terminal commands to build, start, and stop the app, verifies them against the local URL, and records them.

### setup-local-db

For apps that use a database and define its schema in the repo. Creates a local database from that schema, fills it with generated mock data or data copied from an existing database, and points the app at it through an untracked override.

### setup-playwright

Installs [playwright-cli](https://github.com/microsoft/playwright-cli) globally if it's missing, then gets it driving the running app on localhost, read-only until a local database exists.

### setup-security-scan

Gets the security scanners already installed on the machine, such as gitleaks and Snyk, running against the repo, then writes an untracked `security-scan` project skill that reruns them and reports findings by severity with secrets redacted. Installs nothing and asks before any scan that uploads source.

### setup-machine

Not tied to a repo. Makes a requested change to your user-level environment (shell, tools, runtimes, environment variables, Claude config), or audits it for duplicates, hardcoded paths, dead entries, and inconsistencies and proposes fixes. Shows each change before applying it, backs up what it edits, and stays out of admin and system-wide settings.
