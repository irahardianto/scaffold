# Gemini Scaffold

This repository contains the configuration and skills for the Gemini CLI agent. It provides a structured way to plan and execute software engineering tasks using specialized commands.

## Overview

The scaffold defines a set of "skills" and "commands" that the agent can use to standardise workflows.

### Core Commands

1.  **`plan`**: Analyzes requirements and creates a detailed, step-by-step implementation plan.
    -   **Usage**: Used when you need to break down a feature request, bug fix, or PRD into actionable engineering tasks.
    -   **Output**: A Markdown file (usually in `docs/plans/`) containing bite-sized tasks.

2.  **`execute`**: Executes an existing implementation plan.
    -   **Usage**: Used to take a plan file (created by the `plan` command) and implement the code changes in controlled batches.
    -   **Process**: Reads the plan, performs the tasks, verifies with tests, and updates the status.

## Prerequisites

To use this scaffold effectively, you need:

1.  **Gemini CLI**: The core interface for running the agent.
2.  **Archon**: Required for hand-picked languages/libraries knowledge base, Archon's task management is not being used.
3.  **Serena**: The primary tools to allow agents to do surgical precision execution with symbol based file scans & changes.
    *   *Note:* Ensure Serena is activated at the start of your session. The skills defined here rely on Serena's context and capabilities.

## Directory Structure

-   `.gemini/commands/`: specific command definitions (TOML files).
-   `.gemini/skills/`: Detailed instructions and protocols for each skill (Markdown files).

## Getting Started

To use these commands with the Gemini CLI:

1.  **Planning**:
    When you have a new task, tell the agent to "plan" it.
    *Example:* "Plan the user authentication feature."

2.  **Executing**:
    Once a plan exists, tell the agent to "execute" it.
    *Example:* "Execute the plan for user authentication."

The agent will follow the strict protocols defined in the `SKILL.md` files to ensure high-quality, tested, and architecturally sound output.