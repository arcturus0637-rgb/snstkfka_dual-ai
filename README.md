# snstkfka_dual-ai

An explicit-only Codex skill for coordinating **Codex and Claude Code through [OpenRig](https://github.com/mvschwarz/openrig)**. Codex leads frontend and design; Claude Code leads backend and server logic. Each lead can use compatible skills and delegate independent work to agents running the same tool.

This repository contains a workflow skill, not an executable agent runner. Install OpenRig and authenticate both coding tools separately. The skill never starts a team merely because it was installed or mentioned in documentation.

## Default responsibilities

| Lead | Responsibility | Optional parallel workers |
| --- | --- | --- |
| Codex | UI/UX, frontend, responsive behavior, accessibility and visual verification | Codex workers for independent screens, components and frontend checks |
| Claude Code | APIs, backend services, data, authentication and backend tests | Claude Code workers for independent backend areas |

The user can override these roles. Shared contracts come first; file ownership, integration and review remain explicit. A Codex worker renamed “Claude” does not count as running Claude Code.

## Requirements

- A working [OpenRig installation](https://github.com/mvschwarz/openrig#install-and-first-run) in macOS, Linux or WSL2.
- Node.js and tmux versions supported by the installed OpenRig release.
- Codex and Claude Code installed and signed in **in the environment that will run the agents**.
- OpenRig's official [`rigs` skill](https://github.com/mvschwarz/openrig/tree/main/skills/rigs). Follow its current setup and launch guidance; it is not bundled here.

Windows and WSL have different home directories, installed tools and authentication state. Verify access in the selected WSL environment rather than assuming that a working Windows login is sufficient.

## Install in Codex

Use Codex's bundled skill installer if it is available. It refuses to overwrite an existing `dual-ai` directory.

**PowerShell**

```powershell
$codexDir = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $HOME '.codex' }
python (Join-Path $codexDir 'skills/.system/skill-installer/scripts/install-skill-from-github.py') --repo arcturus0637-rgb/snstkfka_dual-ai --path skills/dual-ai
```

**macOS / Linux / WSL**

```bash
python3 "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-installer/scripts/install-skill-from-github.py" \
  --repo arcturus0637-rgb/snstkfka_dual-ai --path skills/dual-ai
```

Alternatively, download or clone this repository and copy the entire `skills/dual-ai` folder into your Codex skills directory, preserving `agents/openai.yaml`. Review or back up an existing installation before replacing it. Start a new Codex chat after installation.

Installing this skill does not install OpenRig, sign either provider in, modify provider permissions, or start any agent.

## Use

Explicitly invoke the skill in Codex with a concrete task:

```text
$dual-ai Implement a login page and its authentication API in this repository.
Codex owns the frontend, Claude Code owns the backend.
Agree on the API contract first, then build and verify both sides.
```

The skill follows this workflow:

1. Read the current project guidance and verify the actual OpenRig environment, tool availability and provider logins.
2. Reuse the official OpenRig workflow to propose a concrete team, runtime assignments and file ownership.
3. Explain launch effects and follow applicable approval requirements; an existing approval for the same launch is reused.
4. Coordinate work through OpenRig, pass readable skill locations to workers, review results and integrate changes.
5. Report what each tool actually did, what was verified and any unresolved failures.

Related fixes and verification stay within the explicitly activated task. Unrelated tasks do not activate mixed execution automatically. Asking to explain, edit, install or publish this skill does not start the team.

## Skills and boundaries

- Each lead can use its own installed global, project and active plugin skills, subject to their invocation rules.
- Design follows the user's current design guidance and existing stack; no particular design library is required.
- Subscription authentication is reused when configured; the workflow must not silently switch to API-key billing.
- Existing teams, user configuration and stricter permission policies must be respected.
- Sending messages, publishing changes, payments and live trading require their own authorization; invoking this workflow grants no blanket permission.
- Provider limits or login failures are reported instead of silently replacing a tool and claiming both ran.

The Codex metadata sets `allow_implicit_invocation: false`. The included workflow instructions are written in Korean; repository documentation and metadata summaries are in English.

## Repository layout

```text
skills/dual-ai/
├── SKILL.md             # Portable workflow instructions
└── agents/openai.yaml   # Explicit-only Codex invocation policy
```

## Verification

The release package was checked for valid Codex skill metadata, explicit-only invocation, portable paths and accidental inclusion of personal configuration or credentials. No OpenRig team was launched to publish this repository. A successful end-to-end mixed-agent run still depends on the installed OpenRig version, both provider accounts, available capacity and the target project.

## License and attribution

[MIT](LICENSE) for this repository's custom skill and documentation. OpenRig and its official skills are separate dependencies with their own licenses. No upstream `rigs` skill, provider binary, private configuration or authentication data is redistributed here.

This is an independent community workflow, not an official OpenRig, OpenAI or Anthropic product.
