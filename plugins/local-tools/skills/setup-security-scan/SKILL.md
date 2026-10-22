---
name: setup-security-scan
description: Use when setting up a repo so Claude can run the machine's installed security scanners (such as gitleaks and Snyk) on it locally, ending with a local security-scan project skill that runs them and reports the findings.
---

# Set up security scans

Gets the installed scanners running against this repo, then writes a local `security-scan` project skill that runs them again and reports the findings in the response.

## Approach
1. Check which scanners are installed and on PATH. If one is missing, tell the user; don't install anything.
2. Learn each tool's commands from its `--help` rather than assuming them.
3. Check whether the repo already has scanner config or a CI scan step. If it does, match its settings.
4. Check that each tool that needs it is authenticated. If it isn't, tell the user to sign in; never handle tokens.
5. Decide the scope with the user: the working tree, git history, dependencies, or source code. Ask before any scan that uploads source to an external service.
6. Show the commands before running them.
7. Run each scan, then check `git status` for anything it wrote (see Output).
8. Summarize the findings by severity and location.
9. Write the project skill (see The security-scan skill), excluding its path first.
10. Verify: invoke `security-scan`, check that it reports the same findings, and check that `git status` lists nothing new.

## Output
- Pick flags that print results to stdout and redact secrets. Don't pass report-file options.
- If a tool still writes into the repo (a cache, report, or policy file), add that path to `.git/info/exclude` (the shared git dir in a worktree) before the next run. Record the path in the project skill.

## The security-scan skill
Write `.claude/skills/security-scan/SKILL.md`. It contains:
- A description that triggers on requests to scan the repo for secrets or vulnerabilities.
- The exact commands and flags, the scope agreed in step 5, and the user's approval for any upload scan.
- How to read each tool's output and exit codes.
- How to report: findings in the response, grouped by severity, each with tool, rule or package, and file and line. Secret values redacted.
- The rules: create no files, fix nothing, run nothing beyond the recorded commands. If a tool reports it isn't signed in, tell the user to sign in.

## Gotchas
- A scanner finding something usually exits non-zero. gitleaks exits 1 when it finds leaks; `snyk test` exits 1 for vulnerabilities, 2 for a failure, and 3 when it finds no supported project. Only the failure codes are errors.
- `snyk monitor` uploads a dependency snapshot to the Snyk account, and `snyk ignore` writes a `.snyk` policy file into the repo. Use neither.

## Guardrails
- Redact secrets in output. Never print, copy, or record a found secret's value.
- Don't add scanner config, ignore files, or CI steps to the repo.
- Report findings; don't fix them as part of setup.
- Don't create or edit CLAUDE.md.
- Don't stage or commit anything. The project skill and anything the scanners write stay untracked, excluded through `.git/info/exclude`, until the user explicitly asks to commit them.

## Done when
Invoking `security-scan` runs each installed scanner, reports the findings in the response, and leaves `git status` unchanged.
