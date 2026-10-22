---
name: setup-machine
description: Use when setting up or cleaning up this machine's user-level environment, including shell, tools, runtimes, environment, and Claude config.
---

# Set up and clean up the machine

Keeps the user-level environment simple, consistent, and easy to reason about. Either makes a requested change, or audits and proposes cleanup.

## Modes
- **Set up:** the user asks for a specific change. Go straight to Changes.
- **Clean up:** the user asks to tidy something. Audit first, then propose.

## What clean looks like
- **One source of truth.** Each tool, runtime, setting, or rule has one owner and one home. Duplicates and competing versions are findings. E.g. several Python installs where one manager like uv should own it; two skills that do the same job.
- **Resolve, don't hardcode.** Things are found the normal way for their kind, not through workarounds that bypass it. E.g. a script called by full path instead of by name.
- **Nothing dead.** Every entry is used, points at something that exists, and still has a reason to be there. E.g. a PATH entry for an uninstalled tool; an alias nothing calls.
- **Small and non-overlapping.** Config and instructions say each thing once, in the right place, and no more than needed. E.g. a long global CLAUDE.md that repeats what skills already cover.
- **Consistent.** Similar things are set up the same way. E.g. some CLIs installed per-user and others globally, for no reason.

Examples show the kind of problem, not a checklist. Look for anything that breaks these principles.

## Clean up
1. Split the environment into areas. For more than one, give each to a read-only subagent in parallel. Subagents report findings; they never change anything.
2. Each finding names what's wrong against the list above, where, why it matters, and the proposed fix.
3. Combine the findings into one list, highest value first, and drop anything trivial.
4. Let the user pick which fixes to make, then apply them per Changes.

## Changes
1. Read the current state first.
2. Show the exact change before and after. Wait for approval.
3. Back up anything before editing it, and say where the backup is.
4. Apply one change at a time.
5. Verify that it took effect and that nothing depending on the old state broke.

## Guardrails
- User scope only. No admin rights or system-wide settings.
- Before removing anything, check what still depends on it.
- Never store secrets in config or the environment.
- Don't touch repos; the local-setup skills handle those.
