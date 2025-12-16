---
name: executing-plans
description: Executes implementation plans in controlled batches with checkpoint reviews. Loads plan files, critically reviews tasks, implements code changes using symbol-level precision, runs verifications, and reports progress between batches. Use when asked to 'execute a plan', 'implement tasks from plan', 'run the implementation', or when working with existing plan files that need execution.
---

# Executing Plans

## Overview

Load plan, review critically, execute tasks in batches, report for review between batches.

**Core principle:** Batch execution with checkpoints for architect review.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

## SEQUENTIAL Task Execution (Do not proceed until current Task is complete)

**CRITICAL:** Before doing anything else, make sure Serena is activated and project is properly onboarded

### 0. Analyze Current State and Plan for Execution
1. READ memory files first: USE Serena and **SWITCH** to planning mode, READ memories to understand the project current state.
2. PARSE the provided plan file
3. UNDERSTAND architecture: "Based on our architecture, where should [feature] live?"
4. FIND similar patterns: "Find existing services similar to what we need"
5. **CRITICAL:** USE technical-constitution skill, it is **SUPREME** and mandatory to follow.

### 1. Review Plan
1. Review critically 
   - [ ] Identify any questions or concerns about the plan
   - [ ] Implementation gap
   - [ ] Dependencies needed
   - [ ] Potential pitfalls and edge cases
2. If concerns: Raise them with your human partner before starting
3. If no concerns: Create write_todos and proceed

### 2. Execute Batch
**Default: First 3 tasks**

**CRITICAL:** Switch Serena to editing and interactive mode

For each task:
1. Mark as in_progress
2. If develop frontend: USE frontend-design skill
3. **Critical:** Before executing the task, RESEARCH using knowledge-searching skill to retrieve language/library/framework details related to the requirements implementations. **Research ONLY information directly relevant to implementing the current task.**
4. Follow each step exactly (plan has bite-sized steps), USE Serena tools for Symbol-level precision editing.  
   - `find_symbol()` -> Performs a global (or local) search for symbols with/containing a given name/substring (optionally filter ed by type).
   - `find_referencing_symbols()` -> Finds symbols that reference the symbol at the given location (optionally filtered by type).
   - `insert_after_symbol` ->  Inserts content after the end of the definition of a given symbol.
   - `insert_before_symbol` -> Inserts content before the beginning of the definition of a given symbol.
   - `replace_symbol_body` -> Replaces the full definition of a symbol.
   - `get_symbols_overview` -> Gets an overview of the top-level symbols defined in a given file.
   - `rename_symbol` -> Renames a symbol throughout the codebase using language server refactoring capabilities.
5. Run verifications as specified
6. Run Codebase investigator subagent to making sure feature already implemented according to plan
7. Mark as completed

### 3. Report
When batch complete:
- Show what was implemented
- Show verification output
- Say: "Ready for feedback."

### 4. Continue
Based on feedback:
- Apply changes if needed
- Execute next batch
- Repeat until complete

### 5. Complete Development

After all tasks complete and verified:
- [ ] RUN Codebase investigator subagent for the FINAL check making sure feature already implemented according to plan.
- [ ] Announce: "Plan execution completed"
   1. Show the summary of what was implemented.
   2. Show the summary of test and verification completed.
   3. How the user can run the applications.
   4. Test scenario for the user to manually test these changes.
- [ ] Update Serena memories: 
   - Focus on CRITICAL project-specific informations that is paramount to reflect the current project state after the changes.
   - Indispensible project-specific information for other AI agents to do their subsequent planning/development/testing.

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker mid-batch (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 0) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- technical-constitution skill is SUPREME, ALWAYS follow what is written there. 
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Between batches: just report and wait
- Stop when blocked, don't guess