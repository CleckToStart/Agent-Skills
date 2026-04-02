---
name: commit-structure
description: '**WORKFLOW SKILL** — Takes synthesized project architecture and rules from the ContextReader agent and persists them. USE FOR: initializing project blueprints; saving tech stack details; establishing AI coding conventions. DO NOT USE FOR: saving daily TODOs or dynamic session progress (use memory skill instead). Outputs exclusively to the `.github/projectStructure/` directory.'
---

# Project Structure Committer

## Execution Steps

When the `ContextReader` agent hands off the synthesized project context to this skill:

### 1. Validate Input
Ensure you have received the structural context (Blueprint, Architecture, Conventions) from the agent.

### 2. Setup Directory
Ensure the directory `.github/projectStructure/` exists.

### 3. Write Structural Files
Write the provided context into the following specific files. **Overwrite** existing content if the architecture has fundamentally changed.

**File 1: `.github/projectStructure/blueprint.md`**
- Content: Tech stack, core dependencies, and high-level architecture diagram/description.

**File 2: `.github/projectStructure/ai_conventions.md`**
- Content: Global coding standards, library preferences, formatting rules, and language requirements specific to this codebase.

### 4. Output Confirmation
```markdown
🏗️ **Project Structure Committed**
The core blueprint and conventions have been successfully saved to `.github/projectStructure/`.
- `blueprint.md`
- `ai_conventions.md`