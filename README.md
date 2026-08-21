# PRD Creator — opencode Agent

A **primary agent for [opencode](https://opencode.ai)** that turns a raw idea into an
**execution-ready PRD + task breakdown**, entirely inside your terminal.

The agent runs a structured Q&A (rendered as interactive popups via the `question` tool),
then writes a consistently structured PRD to `requirement-prd/PRD.md`, optionally a task
breakdown to `requirement-prd/TASK-PRD.md`, and finally generates a dynamic execution prompt
you can hand to any AI coding agent to start development.

## How it works

```
You describe your idea
        │
        ▼
PHASE 1  Context gathering — popup Q&A via the `question` tool
        │  1A Product definition (name, problem, target user)
        │  1B Tech stack preference ("Let AI decide" / "I'll choose")
        │  1C 5–8 project-specific deep questions (skippable)
        ▼
PHASE 2  PRD drafted (never printed in chat) — 8 fixed sections:
        │  Overview · Requirements · Core Features · User Flow
        │  Architecture · Database Schema · Tech Stack · Assumptions & Open Questions
        ▼
FILE     → checks for `requirement-prd/` (asks before overwriting if it exists)
        │  → writes `requirement-prd/PRD.md`
        ▼
PHASE 3  Popup: "Create the task breakdown too?" → yes / no
        ▼
PHASE 4  Task breakdown (Main Feature → Sub Feature → Task, phased & counted)
        │  → writes `requirement-prd/TASK-PRD.md`
        │  → generates a dynamic, imperative execution prompt (code block)
```

## What you get

| Output | Path | Contents |
|---|---|---|
| PRD | `requirement-prd/PRD.md` | 8-section, execution-ready document with acceptance criteria, NFRs, edge cases, Mermaid diagrams, API contract |
| Task breakdown | `requirement-prd/TASK-PRD.md` | 3-level breakdown (feature → sub-feature → task), phased, with `0/N` progress counters |
| Execution prompt | chat (code block) | Imperative prompt referencing both files, phases, and acceptance criteria — paste it into any AI agent |

## Requirements

- opencode (any recent version — the `question` tool is enabled for `app`/`cli`/`desktop` clients by default)
- A model that supports tool calling (the free default model works)

## Installation

### Option A — Global (all projects)

```bash
mkdir -p ~/.config/opencode/agents
cp agents/prd-creator.md agents/prd-reference.md ~/.config/opencode/agents/
```

### Option B — Per project

```bash
mkdir -p .opencode/agents
cp agents/prd-creator.md agents/prd-reference.md .opencode/agents/
```

> Both files **must** live in the same directory — `prd-reference.md` is the structural
> anchor that keeps every generated PRD consistent.

### Enable

1. **Restart opencode** (config is loaded at startup, not hot-reloaded).
2. Press **Tab** to cycle primary agents: `build` → `plan` → `prd-creator`
   (or type `@prd-creator` in chat).

## Usage

1. Switch to **PRD Creator** mode.
2. Describe your idea in plain words.
3. Answer the popup questions (all skippable — skipped answers are marked `[NEEDS INPUT]` in the PRD).
4. Find your PRD at `requirement-prd/PRD.md`, and if you accepted, the task breakdown at `requirement-prd/TASK-PRD.md`.
5. Copy the generated execution prompt into any AI coding agent to start building.

## Files

| File | Purpose |
|---|---|
| `agents/prd-creator.md` | The agent definition (prompt + permissions). |
| `agents/prd-reference.md` | Reference PRD — used **only** as a structural template (headings, formatting, mermaid style). Its content is never copied. |
| `prompt.md` | An install prompt you can paste into any LLM agent to set PRD Creator up for you. |

## Install with any AI agent (prompt)

If you use a CLI AI agent (opencode, Claude Code, Cursor, …), paste the prompt below — it will
install PRD Creator for you:

````text
Install the "PRD Creator" agent from the GitHub repository https://github.com/J-yriz/prd-creator

Steps:
1. Fetch the repository contents (README, agents/prd-creator.md, agents/prd-reference.md).
2. Create the agents directory if missing: ~/.config/opencode/agents (global) or .opencode/agents (project only).
3. Copy agents/prd-creator.md and agents/prd-reference.md into that directory — both files MUST be in the same directory.
4. Do not modify the agent file. Do not use the reference file's content as example output — it is a structural anchor only.
5. Confirm the files exist, then tell the user to restart opencode and switch to the "prd-creator" primary agent (Tab key), and describe an idea to test it.
````

See [`prompt.md`](./prompt.md) for the standalone version.

## Customization

- **Structure:** edit the `STRUCTURAL CONSISTENCY` section of `agents/prd-creator.md` to change PRD sections.
- **Output folder:** edit the `FILE OUTPUT FLOW` section (default: `requirement-prd/`).
- **Model:** the agent uses your default model — add a `model:` line to the frontmatter to pin one.

Open source: free to use, copy, modify, and distribute. **Commercial use is prohibited** —
this project may not be sold or used to generate revenue.