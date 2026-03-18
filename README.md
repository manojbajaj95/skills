# Best Practices

This repo is a home for the defaults I want in every new repository I set up.

It is meant to capture:

- agent setup instructions
- reusable skills
- repo bootstrap checklists
- coding conventions
- workflow defaults
- notes on tools, automation, and operating patterns

## Goal

Instead of re-explaining the same standards in each new project, this repo becomes the source of truth for how I want repos, agents, and supporting workflows to be initialized.

## Installation

### For Humans

Copy and paste this prompt into your LLM agent:

```text
Install and configure this repository's best practices by following the instructions here:
https://raw.githubusercontent.com/manojbajaj95/best-practices/main/installation.md
```

You can read the installation guide yourself, but it is better to let an agent handle the setup.

### For LLM Agents

Fetch the installation guide and follow it:

```bash
curl -s https://raw.githubusercontent.com/manojbajaj95/best-practices/main/installation.md
```

## What Will Live Here

Examples of content that belong in this repo:

- `skills/`: reusable skills for agents
- `agents/`: setup guides, prompts, and operating instructions
- `templates/`: starter files that can be copied into new repos
- `checklists/`: setup and review checklists
- `docs/`: longer-form reference material

## Suggested Structure

```text
.
├── README.md
├── agents/
├── skills/
├── templates/
├── checklists/
└── docs/
```

## Principles

- Keep instructions practical and copy-paste friendly.
- Prefer reusable conventions over one-off notes.
- Write for future setup speed.
- Keep agent guidance explicit: role, constraints, workflow, and expectations.
- Treat this repo as a playbook, not a dumping ground.

## Next Steps

- Add initial agent setup instructions
- Add the first reusable skills
- Add a new-repo setup checklist
- Add starter templates for common repositories

## Status

This is the initial scaffold. The repo will grow into a personal operating manual for repo setup and agent configuration.
