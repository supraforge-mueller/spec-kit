# SDLC Plan: Onboard Unknown Codebase (Mode C)

Use this plan template when starting work on an unfamiliar existing codebase.
After completion, proceed with Mode B (sdlc-add-feature.md) for any features.

## Pipeline Nodes

| # | Phase | Gate Type | Output Artefact |
|---|-------|-----------|-----------------|
| 1 | Code-Analysis | SOFT | `docs/CODEMAPS/` generated |
| 2 | Spec-Gen | SOFT | `.spec/constitution.md`, `.spec/spec.md`, `.spec/plan.md` |
| 3 | Memory-Seed | AUTO | A0 memory entry for this project |

## Gate Definitions

- **SOFT**: Continue even if incomplete — flag gaps, don't block
- **AUTO**: Execute automatically after previous gate passes

## Phase Details

### 1. Code-Analysis
- Tool: `spec_driven_dev:onboard()` → CC `/onboard` skill
- Sub-steps:
  - Run `doc-updater` agent → `docs/CODEMAPS/INDEX.md`
  - Stack fingerprinting (package.json / go.mod / requirements.txt)
  - Extract routes, models, auth patterns, integrations via Grep
- Gate: `docs/CODEMAPS/INDEX.md` exists

### 2. Spec-Gen
- Tool: `spec_driven_dev:onboard()` (continued)
- Sub-steps:
  - Generate `.spec/constitution.md` from observed code patterns
  - Generate `.spec/spec.md` with User Stories from routes/handlers
  - Generate `.spec/plan.md` from directory structure + deps
- Gate: all 3 artefacts exist in `.spec/`

### 3. Memory-Seed
- Action: A0 stores project context in memory
- Content: project name, stack, key files, feature count, architecture summary
- Gate (AUTO): memory entry written

## Output

After successful onboarding:
- `docs/CODEMAPS/` — navigable codebase documentation
- `.spec/constitution.md` — inferred project principles
- `.spec/spec.md` — discovered features as User Stories
- `.spec/plan.md` — architectural overview
- A0 memory entry for cross-session context

## Next Step

Proceed with `sdlc-add-feature.md` to implement features on this codebase.

## Usage

```json
{
  "tool_name": "spec_driven_dev",
  "tool_args": { "method": "onboard" }
}
```
