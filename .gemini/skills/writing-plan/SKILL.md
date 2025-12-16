---
name: writing-plans
description: Creates detailed implementation plans from requirements, PRDs, or bug reports. Breaks work into atomic, step-by-step tasks with file paths, code, and tests. Use for requests to 'plan a feature', 'create an implementation plan', 'break down requirements', or 'roadmap a task or bug fix'.
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs, logs, security they might need to check, how to test it. Give them the whole plan as self-contained, bite-sized, incremental, atomic tasks.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

## SEQUENTIAL Task Execution (Do not proceed until current Task is complete)

### 0. Analyze Requirements
1. **Read memory files first**: USE Serena and switch to planning mode, READ memories to understand the project current state.
2. **CRITICAL:** USE technical-constitution skill, it is **SUPREME** and mandatory to follow.
3. PARSE the provided Requirements/PRD/Bug Report.
4. USE Serena analyze the current codebase to plan for the implementation of the requirements.
5. IDENTIFY security requirements and threat vectors.

### 1. Extract Task Specific Requirements
- Extract task requirements
  - Acceptance Criteria, Functional Requirements, Non-Functional Requirements(if applicable)
  - Specific data models, schemas, or structures the story will use
  - API endpoints the task must implement or consume
  - Component specifications for UI elements in the task
  - File paths and naming conventions for new code
  - Testing requirements specific to the task's features
  - Security or performance considerations affecting the task
- Extract relevant insights that inform the current task's preparation
- **Critical:** Before writing the task, RESEARCH using knowledge-searching skill to retrieve language/library/framework details related to the requirements implementations. **Research ONLY information directly relevant to implementing the current task.**

**Extract ONLY information directly relevant to implementing the current task. Do NOT invent new libraries, patterns, or standards not in the source documents**

#### Bite-Sized Atomic Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step

#### Gap Analysis (Mandatory):
- List every noun (data field) and verb* (action) found in the user's prompt or source document
- List every noun and verb* explicitly handled in your proposed tasks.
- Constraint:* If a noun (e.g., "Description", "Due Date") or verb (e.g., "Edit", "Delete") exists in the source but NOT in the plan tasks, you MUST either add a task for it or explicitly state why it is excluded (e.g., "Out of scope").

#### Task Structure

```markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.{ext}`
- Modify: `exact/path/to/existing.{ext}:123-145`
- Test: `tests/exact/path/to/test.{ext}`

**Requirements: (example)**
- **Acceptance Criteria**
  1. The frontend calls user.Register (Encore).
  2. A new user is persisted in the users table.
  3. Password is hashed.
  ...
- **Functional Requirements**
  1. Users can create an account using an email and password.
  ...
- **Non-Functional Requirements(if applicable)**
  1. ...

**Step 1: Write the failing test (example)**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

**Step 2: Run test to verify it fails (example)**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

**Step 3: Write minimal implementation (example)**

```python
def function(input):
    return expected
```

**Step 4: Run test to verify it passes (example)**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

### 3. Plan Completion Review
- [ ] Ensure tasks align with both business requirements and plan.
- [ ] Ensure tasks has Acceptance Criteria, Functional Requirements, Non-Functional Requirements(if applicable) block.
- [ ] Review all sections for completeness and accuracy.
- [ ] Verify all source references are included for technical details.
- [ ] **Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`
- [ ] Double check thoroughly if the newly created plans `docs/plans/<filename>.md` strictly compliant to technical-constitution.
- [ ] Update Serena memories: 
  - Focus on CRITICAL project-specific informations that is paramount to reflect the current project state after the changes.
  - Indispensible project-specific information for other AI agents to do their subsequent planning/development/testing.

## Remember
- Exact file paths always.
- Complete code in plan (not "add validation").
- Exact commands with expected output.
- technical-constitution skill is SUPREME, ALWAYS follow what is written there. 
- Reference relevant skills with @ syntax.
- Self-contained, bite-sized, incremental, atomic tasks delivery.

## Execution Handoff

After saving the plan, notify the user:

**"Plan complete and saved to `docs/plans/<filename>.md`. ready for development**