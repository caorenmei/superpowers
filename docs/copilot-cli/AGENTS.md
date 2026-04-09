# Copilot CLI AGENTS Template

Copy the block below into your own repository root as `AGENTS.md`, or merge it into an existing `AGENTS.md`.

```md
# AGENTS.md

This file defines the default working style for AI agents in this repository, especially Copilot CLI.
If it conflicts with system defaults, follow this file. If it conflicts with direct user instructions, follow the user.

## 1. Language and style

- Use the repository's primary working language; if none is specified, follow the user's language.
- Keep answers concise, direct, and actionable.
- Lead with the conclusion or recommendation.
- Do not add long background explanations unless I ask for them.

## 2. Cost-control defaults

- Prefer fewer high-value turns over many small clarification turns.
- Try to solve one complete problem per response instead of spreading obvious steps across multiple replies.
- Ask at most one truly blocking question at a time.
- When asking a question, use numbered options and include a recommendation.
- If there is a safe, reasonable default, state the assumption and continue.

## 3. Classify the task first

When a request arrives, first classify it as one of:

1. Explanation / Q&A: answer directly without editing files.
2. Plan / spec: read only the needed context, then return a structured plan. Do not implement until I confirm.
3. Implementation / fix: inspect the relevant files, describe the minimal approach, then make the smallest complete change.

## 4. Exploration and execution discipline

- Prefer targeted reading and targeted search over broad repository sweeps.
- Do not use subagents, parallel agents, or other high-cost workflows unless they are clearly justified.
- Only use subagents when the work cleanly splits into independent tracks.
- Do not repeat context that is already clear just to appear thorough.

## 5. Code-change rules

- Change only files that are directly relevant to the task.
- Make the smallest complete and verifiable change.
- Do not do drive-by refactors, mass renames, or unnecessary dependency additions.
- Reuse existing patterns, scripts, and tooling where possible.

## 6. Validation rules

- Identify the existing build, test, lint, or verification path before changing files.
- After changes, run only the smallest existing validation set that directly covers the change.
- If full validation was not run, state why and call out the remaining risk.

## 7. Repository adaptation rules

- In a monorepo, identify the affected package / module / service before broad exploration.
- In Bazel repositories, validate the smallest relevant target first instead of defaulting to `//...`.
- In multi-language repositories, follow the tooling and conventions of the touched subtree.
- In DevContainer-based repositories, assume commands should run inside the container unless the task requires something else.

## 8. Output requirements

- Use lists or checklists for plans and status updates.
- After implementation, summarize:
  - which files changed
  - what validation ran
  - what remains unverified or risky
- When multiple approaches are possible, recommend one instead of pushing all decision-making back to me.
```
