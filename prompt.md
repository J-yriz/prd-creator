# PRD Creator — Install Prompt

Paste the prompt below into any AI coding agent (opencode, Claude Code, Cursor, …) to
install the **PRD Creator** agent from [https://github.com/J-yriz/prd-creator](https://github.com/J-yriz/prd-creator).

````text
Install the "PRD Creator" agent from the GitHub repository https://github.com/J-yriz/prd-creator

Steps:
1. Fetch the repository contents (README, agents/prd-creator.md, agents/prd-reference.md).
2. Create the agents directory if missing: ~/.config/opencode/agents (global) or .opencode/agents (project only).
3. Copy agents/prd-creator.md and agents/prd-reference.md into that directory — both files MUST be in the same directory.
4. Do not modify the agent file. Do not use the reference file's content as example output — it is a structural anchor only.
5. Confirm the files exist, then tell the user to restart opencode and switch to the "prd-creator" primary agent (Tab key), and describe an idea to test it.
````

## What the agent does once installed

1. Asks structured questions via interactive popups (the `question` tool).
2. Writes a consistently structured PRD to `requirement-prd/PRD.md`.
3. Optionally writes a phased task breakdown to `requirement-prd/TASK-PRD.md`.
4. Generates a dynamic, imperative execution prompt you can paste into any AI agent to start development.