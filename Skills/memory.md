---
name: memory
description: '**WORKFLOW SKILL** — Create, update, review, or establish project context and memory files. USE FOR: initializing AI memory for a new project; saving current development progress; establishing coding conventions; ensuring cross-device and cross-AI session continuity; summarizing active bugs or TODOs. DO NOT USE FOR: general coding tasks; writing application logic; runtime debugging; generating standard README.md files for users. INVOKES: file system tools (read/write memory files in .github/memory/), codebase exploration tools (to understand project state and recent changes). FOR SINGLE OPERATIONS: If the user just wants to quickly note down a single TODO, edit the .github/memory/context_memory.md directly — no full scan needed.'
---

# Project Memory Management

## Decision Flow

| Memory Primitive | When to Use | Update Frequency |
|------------------|-------------|------------------|
| Project Blueprint| Defining core architecture, tech stack, business logic, and directory structure. | **Rare** (Initialization or Major Refactors) |
| Dynamic Context  | Recording current dev progress, active bugs, recent decisions, and immediate TODOs. | **High** (End of a session or task completion) |
| AI Conventions   | Setting specific coding rules, preferred libraries, or prompt preferences for this specific repo. | **Medium** (When new patterns emerge) |
| Snapshot Logs    | (Optional) Archiving a specific milestone before a massive change. | **On-demand** |

## Quick Reference

All memory files must reside in the `.github/memory/` directory to maintain a clean project root and ensure they are automatically synced via Git for cross-device development.

| Type | File | Location | Content Focus |
|------|------|----------|---------------|
| Blueprint | `project_intro.md` | `.github/memory/` | What is this project? (Tech stack, architecture) |
| Dynamic Context | `context_memory.md` | `.github/memory/` | Where are we right now? (Progress, blockers, next steps) |
| AI Conventions | `ai_conventions.md` | `.github/memory/` | How should AI behave here? (Formatting, strict rules) |

## Creation & Update Process

When invoked to manage memory or "save context", use codebase exploration tools to understand the current state, then follow these steps:

### 1. Analyze Current State
- Read package managers (`package.json`, `requirements.txt`, etc.) to identify the tech stack.
- Review recent file modifications and git status (if available) to understand current progress.
- If asked to save a specific conversation context, extract the core technical decisions made in the current chat.

### 2. Ensure Directory Exists
- Check if `.github/memory/` exists in the project root. If not, create it.

### 3. Route & Execute
Update the appropriate primitive based on the user's intent:
- **For full initialization**: Generate all three core files (`project_intro.md`, `context_memory.md`, `ai_conventions.md`).
- **For saving progress**: Only overwrite or append to `.github/memory/context_memory.md`.
- **For setting rules**: Only update `.github/memory/ai_conventions.md`.

### 4. Validate Output
- Ensure the Markdown is clean and highly structured (use lists and bold text for scannability).
- **CRITICAL**: Do not dump raw code into memory files unless it is a vital architectural pattern. Use high-level summaries.

## Edge Cases

**Update vs. Append?** For `context_memory.md`, always maintain a "Current Status" section that gets *overwritten* with the latest state, and a "History/Log" section that gets *appended* to. This prevents the AI from reading outdated "current" statuses.

**Global vs. Local Memory?**
This skill manages *Local Memory* (specific to this repository). If the user asks to save a personal preference (e.g., "Always call me Alex"), inform them that this belongs in their global user settings, not the repo's `.github/memory/`.

## Common Pitfalls

**Memory Bloat.** If `context_memory.md` gets too long, the AI context window will be wasted when reading it. Always summarize and prune resolved bugs or completed TODOs from the active section.

**Silent Failures on Read.** Creating memory is useless if the AI doesn't read it. Always add a directive in `ai_conventions.md` instructing future AI sessions: *"Always read `.github/memory/context_memory.md` before answering the first query in a new session."*

**Assuming the framework.** Don't guess the tech stack based on one file. Verify across the codebase before writing `project_intro.md` to avoid hallucinated architecture documentation.