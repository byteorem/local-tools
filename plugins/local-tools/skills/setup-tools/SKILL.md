---
name: setup-tools
description: Use when checking that the tools a repo needs (build tools, SDKs, runtimes, package managers, CLIs) are installed and usable on this machine, or when a build or script fails with a command not found or wrong version. Run this after setup-local-claude and before setup-local-run.
---

# Set up tools

Checks that the tools this repo needs are installed, usable, and the right version. Reports what's off; installs nothing.

## Approach
1. Check CLAUDE.local.md for tools already recorded. If they're there, verify them instead of starting over. If CLAUDE.local.md doesn't exist, run setup-local-claude first.
2. Detect the stack from the repo: solution and project files, target frameworks, manifests, lockfiles, version pins, scripts, deploy workflows.
3. List the tools the repo needs and why: build tool, package restore, runtime, local server, package manager, CLIs its scripts or workflows call.
4. Check each one from the repo root: it's on PATH, it runs, and its version satisfies what the repo pins or targets.
5. If a tool isn't on PATH, look where its installer usually puts it, e.g. find MSBuild through vswhere. Note the full path.
6. Report each tool as ok, missing, wrong version, or found but not on PATH, with why the repo needs it and what the user would do to fix it.
7. Record in CLAUDE.local.md the tools to use and any full paths or non-default ways to call them.

## Gotchas
- Run version checks from the repo root. Pin files such as `global.json`, `.nvmrc`, `.python-version`, and `.tool-versions` change which version a tool resolves to in that directory.
- A pin is often a range, not an exact version. `global.json` has `rollForward`, `engines` takes semver ranges, and `packageManager` is enforced through corepack. Judge "wrong version" by the pin's own rules.
- Lockfiles say which package manager the repo uses: `bun.lock`, `pnpm-lock.yaml`, `yarn.lock`, `package-lock.json`, `uv.lock`, `poetry.lock`. Check that one, not whichever is installed.
- Don't check a tool with `npx`, `bunx`, `uvx`, or `dotnet tool run` when it isn't installed. They download and run it, which is an install.
- On Windows, `python` and `python3` can resolve to Store aliases under `WindowsApps` that open the Store instead of running Python. Treat that as missing.
- vswhere lives at `%ProgramFiles(x86)%\Microsoft Visual Studio\Installer\vswhere.exe` even when it's not on PATH. `vswhere -latest -requires Microsoft.Component.MSBuild -find MSBuild\**\Bin\MSBuild.exe` finds MSBuild.

## Guardrails
- Read-only on the machine. Don't install, update, or change PATH or system settings. Tell the user what's off and let them fix it, since installs may need approval.
- Only check tools the repo actually needs. Don't audit the whole machine.
- Don't create or edit CLAUDE.md.
- Don't stage or commit anything. CLAUDE.local.md stays untracked, excluded through `.git/info/exclude`, until the user explicitly asks to commit it.

## Done when
Every tool the repo needs is listed with its status, CLAUDE.local.md records how to call each working one, and the user knows what to fix.
