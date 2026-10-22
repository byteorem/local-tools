# work-mode

> Claude Code skills for setting up a repo so the agent can run, seed, drive, and scan the app locally

Everything these skills set up is local to your machine. Each skill walks Claude through one setup step, shows you the plan before acting, and records the result in `CLAUDE.local.md` or a local project skill. Every file they create stays untracked through `.git/info/exclude` and is never committed unless you ask. Nothing touches tracked files, shared databases, or credentials.

## Install

```
/plugin marketplace add byteorem/work-mode
/plugin install work-mode@work-mode
```

## Usage

Run the skills in order inside the repo you want to set up:

```
/work-mode:setup-local-claude
/work-mode:setup-tools
/work-mode:setup-local-run
/work-mode:setup-local-db
/work-mode:setup-playwright
/work-mode:setup-security-scan
```

## Skills

### setup-local-claude

Creates or extends `CLAUDE.local.md` and local `.claude/` config (`settings.local.json`, project skills, agents) with how to work with the repo on this machine, and keeps them untracked.

### setup-tools

Checks that the build tools, SDKs, runtimes, package managers, and CLIs the repo needs are installed and match its version pins, finds any that are installed but off PATH, and records how to call them. Installs nothing.

### setup-local-run

Finds the terminal commands to build, start, and stop the app, verifies them against the local URL, and records them.

### setup-local-db

Builds a local database from the repo's own schema, seeds it with generated mock data, and points the app at it through an untracked override.

### setup-playwright

Installs [playwright-cli](https://github.com/microsoft/playwright-cli) globally if it's missing, then gets it driving the running app on localhost, read-only until a local database exists.

### setup-security-scan

Gets the security scanners already installed on the machine, such as gitleaks and Snyk, running against the repo, then writes an untracked `security-scan` project skill that reruns them and reports findings by severity with secrets redacted. Installs nothing and asks before any scan that uploads source.
