# SDLC Plan: Add Feature to Existing Project (Mode B)

Use this plan template when adding a feature to an existing codebase.

## Pipeline Nodes

| # | Phase | Gate Type | Output Artefact |
|---|-------|-----------|-----------------|
| 1 | Read-Context | SOFT | `.spec/` + CODEMAPS verified (auto-onboards if missing) |
| 2 | Spec | SOFT | `.spec/spec.md` (feature-scoped) |
| 3 | Plan | SOFT | `.spec/plan.md` |
| 4 | Tasks | SOFT | `.spec/tasks.md` |
| 5 | Implement | HARD | all tasks complete |
| 6 | Build + Test + Security | HARD | green + no CRITICAL |
| 7 | Deploy | APPROVAL | human verifies |

## Gate Definitions

- **SOFT**: Continue even if incomplete — flag gaps, don't block
- **HARD**: Must pass before proceeding — retry/fix until green
- **APPROVAL**: Pause and present URL to human for sign-off

## Phase Details

### 1. Read-Context
- Tool: `spec_driven_dev:read_context()`
- If `.spec/constitution.md` and `docs/CODEMAPS/INDEX.md` both exist → context loaded
- If either missing → auto-triggers `spec_driven_dev:onboard()`
- Gate: context loaded with stack + key files identified

### 2. Spec
- Tool: `spec_driven_dev:spec(topic=<feature_description>)`
- Must respect existing constitution principles
- Gate: spec.md with user stories tied to existing system

### 3. Plan
- Tool: `spec_driven_dev:plan()`
- Must reference existing architecture from CODEMAPS
- Gate: plan.md with concrete file paths in existing project structure

### 4. Tasks
- Tool: `spec_driven_dev:tasks()`
- Gate: tasks.md with at least 3 tasks

### 5. Implement
- Tool: `spec_driven_dev:implement(task_id=<id>)` — repeat for all tasks
- Gate (HARD): all tasks complete

### 6. Build + Test + Security
- Tools: `spec_driven_dev:build()` → `spec_driven_dev:test()` → `spec_driven_dev:security()`
- Gate (HARD): all three pass

### 7. Deploy
- Tool: `spec_driven_dev:deploy(env=preview)`
- Gate (APPROVAL): present URL to human

## Usage

```json
{
  "tool_name": "spec_driven_dev",
  "tool_args": { "method": "read_context" }
}
```

After context loaded: spec(topic=<feature>) → plan → tasks → implement → build → test → security → deploy.
