---
name: setup-local-claude
description: Use when creating or updating a repo's local-only Claude Code setup on this machine, such as CLAUDE.local.md, .claude/settings.local.json, or project skills, agents, and commands under .claude/ that shouldn't be shared.
---

# Set up local Claude config

Covers how to work with this repo on this machine: CLAUDE.local.md and anything under `.claude/`. All of it stays untracked. Committing it is the user's decision, made explicitly; once it's in the repo, everyone with access gets it.

## Approach
1. Check for an existing CLAUDE.local.md, CLAUDE.md, and `.claude/` directory, and which of those files git tracks (`git ls-files CLAUDE.md CLAUDE.local.md .claude`). Extend local files rather than replacing them, and don't repeat what CLAUDE.md already says.
2. Detect the stack from what the repo contains: solution and project files, manifests, config, deploy workflows. State what you found.
3. Draft only what the repo confirms or the user tells you. Ask about gaps that matter; leave out the rest.
4. Show the draft before writing.
5. Exclude each new path from git (see Git), then write it.
6. Verify that `git status` lists none of the new files.

## What belongs in CLAUDE.local.md
- How to run the app locally: command or IDE steps, ports, local URLs.
- Local dependencies (database, services) and which environment each one points at.
- The auth method for local runs, never the credentials.
- Personal working rules, e.g. "no DB access; return SQL for the user to run."
- Anything about this machine's setup that the repo doesn't record.

## What doesn't
- Secrets, passwords, connection strings with credentials, real user or business data.
- Never create or edit CLAUDE.md. If a fact would belong there, put it in CLAUDE.local.md.
- Rules true for every repo. Those go in the global CLAUDE.md.
- General framework knowledge.

## .claude/ config
- Put permissions, env vars, and other settings in `.claude/settings.local.json`, never `.claude/settings.json`.
- Project skills, agents, commands, and hooks written for local use go under `.claude/` and get excluded like everything else.
- Don't edit tracked files under `.claude/`. If a change seems to need one, stop and ask.

## Git
- Never stage or commit. Everything this skill writes stays untracked until the user explicitly asks to commit it.
- Exclude through `.git/info/exclude`, not `.gitignore`, so no tracked file changes. Exclude the specific paths you create, not all of `.claude/`, so new files meant for the repo still show up in `git status`.
- In a worktree, write the exclude to the shared git dir (`git rev-parse --git-common-dir`). It then applies to every worktree.
- If a file is already tracked, stop and ask.

## Style
- Present tense and imperative. Point to paths instead of copying code.
- Keep CLAUDE.local.md under about 60 lines.
