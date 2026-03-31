# SDLC Plan: From Scratch (Mode A)

Use this plan template when building a new application from scratch.

## Pipeline Nodes

| # | Phase | Gate Type | Output Artefact |
|---|-------|-----------|-----------------|
| 1 | Discover | SOFT | `.spec/research.md` |
| 2 | Scaffold | SOFT | project skeleton + git init |
| 3 | Spec | SOFT | `.spec/spec.md` |
| 4 | Plan | SOFT | `.spec/plan.md` |
| 5 | Tasks | SOFT | `.spec/tasks.md` |
| 6 | Implement | HARD | all tasks complete, artefacts verified |
| 7 | Build + Test | HARD | build OK, coverage ≥80% |
| 8 | Security | HARD | no CRITICAL issues |
| 9 | Deploy | APPROVAL | human verifies deployed URL |
| 10 | Monitor | AUTO | 4 cron tasks created |

## Gate Definitions

- **SOFT**: Continue even if incomplete — flag gaps, don't block
- **HARD**: Must pass before proceeding — retry/fix until green
- **APPROVAL**: Pause and present URL to human for sign-off
- **AUTO**: Execute automatically after previous gate passes

## Phase Details

### 1. Discover
- Tool: `/discover` CC skill (invoked directly — no A0-side `discover` method)
- Outputs: `.spec/research.md` with competitor analysis, TAM/SAM, recommended stack
- Gate: research.md exists with at least 3 competitors

### 2. Scaffold
- Tool: `/scaffold` CC skill
- Outputs: project directory, package files, Dockerfile, CI config, `.spec/` stub
- Gate: `git log` shows initial commit

### 3. Spec
- Tool: `spec_driven_dev:spec(topic=<app_description>)`
- Outputs: `.spec/spec.md` with user stories + acceptance criteria
- Gate: at least 3 user stories with Given-When-Then ACs

### 4. Plan
- Tool: `spec_driven_dev:plan()`
- Outputs: `.spec/plan.md` with architecture, data model, API contracts
- Gate: plan.md exists and references spec user stories

### 5. Tasks
- Tool: `spec_driven_dev:tasks()`
- Outputs: `.spec/tasks.md` with [P] parallelization markers
- Gate: at least 5 tasks defined

### 6. Implement
- Tool: `spec_driven_dev:implement(task_id=<id>)` — repeat for all tasks
- Gate (HARD): all `- [ ]` in tasks.md changed to `- [x]`

### 7. Build + Test
- Tools: `spec_driven_dev:build()` → `spec_driven_dev:test(coverage_threshold=80)`
- Gate (HARD): build exits 0 AND coverage ≥80%

### 8. Security
- Tool: `spec_driven_dev:security()`
- Gate (HARD): no CRITICAL severity issues

### 9. Deploy
- Tool: `spec_driven_dev:deploy(env=preview)`
- Gate (APPROVAL): present URL to human, wait for explicit approval

### 10. Monitor
- Tool: `spec_driven_dev:monitor_setup()`
- Gate (AUTO): 4 scheduled tasks created in A0 scheduler

## Usage

```json
{
  "tool_name": "spec_driven_dev",
  "tool_args": { "method": "spec", "topic": "<app description>" }
}
```

After spec: proceed through plan → tasks → implement → build → test → security → deploy → monitor_setup.
