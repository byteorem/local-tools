---
name: setup-local-claude
description: Use when creating or updating a repo's CLAUDE.local.md, the local-only instructions for working with that repo on this machine.
---

# Set up CLAUDE.local.md

CLAUDE.local.md covers how to work with this repo on this machine. It is never committed.

## Approach
1. Check for an existing CLAUDE.local.md and CLAUDE.md. Extend the local file rather than replacing it, and don't repeat what CLAUDE.md already says.
2. Detect the stack from what the repo contains: solution and project files, manifests, config, deploy workflows. State what you found.
3. Draft only what the repo confirms or the user tells you. Ask about gaps that matter; leave out the rest.
4. Show the draft before writing.
5. Write the file and exclude it from git (see Git).
6. Verify that `git status` does not list the file.

## What belongs
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

## Git
- Use `.git/info/exclude`, not `.gitignore`, so no tracked file changes.
- In a worktree, write the exclude to the shared git dir (`git rev-parse --git-common-dir`). It then applies to every worktree.
- If the file is already tracked, stop and ask.

## Style
- Present tense and imperative. Point to paths instead of copying code.
- Keep it under about 60 lines.
