# Superpowers for GitHub Copilot CLI

Guide for using Superpowers with GitHub Copilot CLI, with an emphasis on low-friction repository instructions and cost control.

## Why this document exists

Copilot CLI can follow repository-level instructions well, but a verbose `AGENTS.md` often creates three avoidable costs:

1. Too many clarification turns before the agent starts useful work
2. Over-broad exploration and validation
3. Expensive subagent or deep-workflow usage for tasks that do not need it

This guide pairs a **short, copy-pasteable `AGENTS.md` template** with the reasoning behind it, so you can keep the repo instruction file small while moving the long explanation into docs.

## Files in this repo

- Short template: [`docs/copilot-cli/AGENTS.md`](copilot-cli/AGENTS.md)
- This guide: `docs/README.copilot.md`

Recommended usage:

1. Copy the template into your own repository root as `AGENTS.md`
2. Trim or extend only the sections that match your workflow
3. Keep the root `AGENTS.md` short; put longer rationale in docs like this one

## Design goals

The template is optimized for repositories that want instructions to be:

- **Generic first** — useful beyond one company, one stack, or one team
- **Cheap to follow** — fewer premium turns, fewer unnecessary tool calls, less context churn
- **Safe for implementation** — minimal changes, clear validation expectations
- **Copilot CLI-friendly** — assumes the agent can search, edit, run shell commands, and optionally dispatch subagents, but should not do the expensive thing by default

## What the short template is trying to enforce

### 1. Classify the task before acting

The biggest waste pattern is treating every request like an implementation request.

The template tells the agent to separate:

- explanation / Q&A
- plan / spec
- implementation / bug fix

That avoids needless file edits, needless repo exploration, and needless validation for answer-only requests.

### 2. Ask fewer, better questions

For Copilot CLI specifically, costs go up when the session degenerates into many small turns.

The template therefore pushes the agent to:

- ask only one blocking question at a time
- use numbered options
- recommend one option
- continue under a stated default when safe

This reduces both token churn and decision fatigue.

### 3. Prefer targeted reads over broad sweeps

The template discourages:

- scanning the whole repo before understanding the task
- broad multi-agent exploration
- full-repo validation by default

Instead it pushes the agent toward:

- reading only relevant files
- narrowing scope early
- validating the smallest affected surface first

### 4. Reserve subagents for genuinely parallel work

Copilot CLI can dispatch extra agents, but that should be treated as an exception, not the default.

Use them when the task cleanly splits into independent tracks. Avoid them for:

- simple edits
- focused debugging
- single-module changes
- work where the coordination overhead exceeds the benefit

### 5. Keep implementation local and narrow

The template tries to prevent common agent failure modes:

- drive-by refactors
- whole-file reformatting unrelated to the task
- unnecessary dependency additions
- validation scope that is much larger than the change scope

## Optional customizations

The template is intentionally generic. Add only the pieces your repository really needs.

### If your repo is a Bazel monorepo

You can keep the short template as-is and rely on the built-in lines about monorepos and Bazel. If you want to be stricter, append rules like:

- identify the affected target before editing
- prefer `bazel query` and narrow target tests
- do not run `bazel test //...` unless explicitly required

### If your repo is multi-language

Append a short section telling the agent to:

- respect per-language tooling
- avoid applying one language's conventions to another
- validate only the changed language subtree unless a shared boundary was touched

### If your repo uses DevContainer

Append a short section telling the agent to:

- assume the container is the default execution environment
- avoid host-only setup advice unless necessary
- avoid changing devcontainer config unless the task requires it

### If your team works in 简体中文

The template already defaults to Simplified Chinese. If you want stricter output control, add lines such as:

- code and commands stay in original language
- explanations stay in Chinese
- answers should start with the conclusion

## Suggested operating policy for Copilot CLI

If you want a stronger cost-control posture, these are good defaults:

1. **Default to a single-agent workflow**
2. **Default to the shortest path that preserves correctness**
3. **Default to one blocking question, not a questionnaire**
4. **Default to minimal relevant validation, not full-repo validation**
5. **Default to explicit assumptions when risk is low**

These defaults usually lower cost more effectively than obsessing over individual tool calls.

## Anti-patterns to avoid

If your current `AGENTS.md` produces expensive sessions, it often contains one of these patterns:

### "Always investigate thoroughly before answering"

This makes even simple questions expensive.

Better: answer directly for Q&A; investigate only when the task actually needs repo context.

### "Always ask for confirmation before proceeding"

This creates unnecessary turns.

Better: ask only when the answer changes the implementation path materially.

### "Always use subagents / always parallelize"

This raises coordination cost quickly in Copilot CLI.

Better: use subagents only when the task genuinely decomposes.

### "Always run full build and full test suite"

This is often disproportionate for documentation or isolated changes.

Better: run the narrowest existing validation that credibly covers the change, then report what remains unverified.

### "Always explain all tradeoffs in detail"

This inflates output size and often does not help the human partner.

Better: give the recommendation first, then only the tradeoffs needed for the current decision.

## A good default customization workflow

When adapting the template for a real repo:

1. Keep the core sections unchanged:
   - language and style
   - cost control defaults
   - task classification
   - exploration discipline
   - change safety
2. Add at most one short repo-specific section for:
   - build system
   - monorepo boundaries
   - environment assumptions
3. Avoid adding long philosophy sections to root `AGENTS.md`
4. Move rationale, examples, and exceptions into separate docs

## Example: when to stay generic vs when to specialize

### Keep in the short template

- use Chinese
- be concise
- classify task type
- ask one blocking question at a time
- prefer narrow exploration
- prefer narrow validation

### Move to detailed docs instead

- exact Bazel command recipes
- full monorepo boundary policy
- language-by-language validation matrix
- DevContainer troubleshooting
- examples of good and bad prompts

## Copying into an existing AGENTS.md

If your repository already has an `AGENTS.md`, do not blindly replace it.

Merge in only the sections that address:

- verbosity
- clarification count
- subagent policy
- validation scope

The biggest win usually comes from deleting redundant prose, not adding more rules.

## README snippet you can reuse

If you want to document this setup for your team, a small README snippet is usually enough:

> We keep `AGENTS.md` short on purpose. It sets language, task classification, cost-control defaults, narrow validation, and subagent policy. Longer rationale belongs in docs, not in the root instruction file.

## Getting Help

- Main documentation: https://github.com/obra/superpowers
- Copilot CLI install section: [README.md](../README.md)
