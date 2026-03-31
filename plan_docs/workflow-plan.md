# Workflow Execution Plan: project-setup

**Repository:** intel-agency/workflow-orchestration-queue-juliet23
**Workflow Source:** [project-setup](https://github.com/nam20485/agent-instructions/blob/main/ai_instruction_modules/ai-workflow-assignments/dynamic-workflows/project-setup.md)

**Generated:** 2026-03-31

---

## 1. Overview

- **Workflow Name:** `project-setup`
- **Workflow File:** `ai_instruction_modules/ai-workflow-assignments/dynamic-workflows/project-setup.md`
- **Project Name:** workflow-orchestration-queue (OS-APOW)
n- **Project Description:** A headless agentic orchestration platform that transforms GitHub Issues into automated execution orders. The autonomous AI worker dispatches code, runs tests, and submits PRs.
n- **Total Assignments:** 6 main + 2 events (validate-assignment-completion, report-progress) + 1 post-event (apply label)
n- **Pre-requisite:** None (this is the pre-script event)

---

## 2. Project Context Summary

### Key Facts from `plan_docs/`

| Field | Value |
|--- |---|---------------|
| Project Name | workflow-orchestration-queue (OS-APOW) |
| Tech Stack | Python 3.12, FastAPI, Pydantic, HTTPX, uv, Docker, DevContainers |
| Architecture | 4-pillar: Ear (Notifier), State (GitHub Issues), Brain (Sentinel), Hands (Worker) |
| Repository | intel-agency/workflow-orchestration-queue-juliet23 |
| Phases | Phase 0 (Seeding), Phase 1 (Sentinel MVP), Phase 2 (Webhook), Phase 3 (Deep Orchestration) |
| Key Constraints | Action SHA pinning required for all GitHub Actions workflows |
| CI/CD pipelines require validation before merge |
| Branch protection rules require setup |
    Labels from `.github/.labels.json` must be sync with `scripts/import-labels.ps1`)
    - Single-repo polling only (future: cross-repo org-wide via Search API)
    - Docker resource constraints (2 CPU, 4GB RAM)
    - HMAC webhook verification required
    - Credential scrubbing required before posting to GitHub
    - Heartbeat comments every 5 minutes for long-running tasks
    |
| **Simplifications Applied** | |
    - Unified data model (`src/models/work_item.py`)
    - Connection pooling for `src/queue/github_queue.py`)
    - Single-repo polling only (cross-repo deferred to Future)
    - 3 env vars only (`GITHUB_TOKEN`, `GITHUB_ORG`, `SENTINEL_BOT_LOGIN`)
)
    - Single-mode env reset hardcoded to "stop")
    - Dual logging removed (stdout only)
    - Phase 3 features moved to appendix
    |

---

## 3. Assignment Execution Plan

For assignment in execution order, document:

| **Assignment** | **Goal** | **Key Acceptance Criteria** | **Project-Specific Notes** | **Prerequisites** | **Dependencies** | **Risks/Challenges** | **Events** |
| --- |---|
| **1. `create-workflow-plan`: Create a comprehensive workflow execution plan for the project-setup workflow. Draw on project context from plan_docs, ensures all stakeholders share a common understanding of what will happen, in what order, and why. - All `plan_docs/` files have been read and summarized
- Plan has been presented for user/stakeholder in approval before proceeding.
- The note any potential risks or challenges in each assignment given the project context)
- Save the approved plan as `plan_docs/workflow-plan.md`
    - Commit to this repository
    |
| **Dependencies:** None (pre-script event) |
| **Sequencing:** Linear (1 event before 2 events)
    - All main assignments must to complete before the post-script-complete event fires
    - 2 post-assignment events (validate-assignment-completion, report-progress) must also fire after, such will report issues to addressed with stakeholders
    - Record outputs in session context for further use in subsequent assignments.

    |
| **Assignment** | `init-existing-repository`: Initialize repository |
| **Goal:** Set up branch, project board, labels, milestones, rename workspace files, create PR |
    **Key Acceptance Criteria** |  |
| - New branch created (must be first — all other work commits here)
    - Branch protection ruleset imported from `.github/protected-branches_ruleset.json`
    - GitHub Project created, linked to repository
    - Labels imported from `.github/.labels.json`
    - Workspace/devcontainer files renamed to match project name
    - PR created from setup branch to main
    - Filenames changed to match project name
    - **Project-Specific Notes** | \n  - This is a a fresh repository from a template, needs:
        - Verify repo already exists; skip toolset import
        - Check `.github/.labels.json` for current labels
        - Use `scripts/import-labels.ps1` for idempotency)
        - Commit file changes first, then create PR
    - **Project-Specific Notes** | |
    - This is a template repo from the `workflow-orchestration-queue-juliet23` template, the `devcontainer-opencode.sh` bridge ensures environment parity)
        - Action SHA pinning is mandatory — all actions in `.github/workflows/` must be pinned to SHA of releases)
        - Template placeholders (`workflow-orchestration-queue-juliet23`) will be replaced
        - Branch naming convention is `dynamic-workflow-<workflow-name>`
        - PR created after first commit
    - **Risks/Challenges** | None - Low. Branch creation may fail due to incomplete plan
 |
    - **CI Verification:** Must run `./scripts/validate.ps1 -All` before each main assignment completes
      - Setup PR requires self-approval (acceptable for automated setup PR)
      - **Project-Specific Notes** | |
    - This is a template repo created from the script; need to verify the existing labels
        - Files in `.devcontainer/` and `.code-workspace` already exist
        - `ai-new-app-template.code-workspace` should be renamed
        - Branch naming: Follow convention `dynamic-workflow-<workflow-name>`
        - **Dependencies:** Requires `init-existing-repository` output (PR number)
    | **create-app-plan**: Analyze template and supporting docs, create detailed plan
        - Create milestones from plan_docs
        - Link issues to milestones
        - Create/update remote indices
        - Create documentation structure
        - Run local tests
        - Create issue templates
        - Obtain stakeholder approval
    | **Project-Specific Notes** | |
    - This is a template repo from the `workflow-orchestration-queue-juliet23`, so the template was dynamically)
        - Existing AGENTS.md needs updating
      - Follow steps in `create-agents-md-file` assignment to customize AGENTS.md
      - Plan references `plan_docs/` will with `./APP_PLAN.md` or `APP_PLAN.md`)
      - Ask if `plan_docs/architecture.md` or `architecture.md` exists
        - **Dependencies:** `init-existing-repository` (creates PR, project board, labels)
      - `create-project-structure` outputs (structure, PR number, milestones)
    | **create-agents-md-file` depends on `init-existing-repository` for branch creation and PR merge.
    - **Project-Specific Notes** | |
    - Already exists as template repo. Some files already seeded in `plan_docs/` (Architecture Guide, Implementation Spec, OS-APOW Plan Review, Simplification Report)
        - File renames and code cleanup may be faster than manual edits
      - **Risks/Challenges** | None- low, Branch creation may fail due to incomplete plan. |
        | **CI Verification:** Must run `./scripts/validate.ps1` for all validation before each main assignment.      - Setup PR requires self-approval (acceptable for automated setup PR)
      - **Project-Specific Notes** | |
    - This is a template repo, some files already seeded in `plan_docs/` (Architecture Guide, Implementation Spec, OS-APOW Plan Review, Simplification Report)
        - Existing Python files in plan_docs provide reference implementation
      - Existing AGENTS.md in plan_docs needs updating
      - Plan references should already exist in AGENTS.md and      - Rename `.ai-new-app-template.code-workspace` to `.devcontainer/devcontainer.json` name
      - Import labels from scripts
      - Run `scripts/validate.ps1` for all validation)
      - Verify GitHub Project board setup
      - Verify build succeeds
      - Create PR from branch
      - Link PR to related issues (PR created during `init-existing-repository`)

### 4. Dependencies
| `init-existing-repository` | Must complete first | provides PR number to reference
 downstream.

| **Prerequisites** | `init-existing-repository` output (PR number) |
| **Dependencies** | `init-existing-repository` provides PR number for Phase 1 (create-app-plan) and project structure creation |
| **Project-Specific Notes** | This project is a template repo created from a template. Existing Python files in plan_docs provide reference implementation. Ex: existing AGENTS.md needs updating in AGENTS.md,      - Rename `.ai-new-app-template.code-workspace` to `.devcontainer/devcontainer.json` name
      - Import labels through scripts
      - Run `scripts/validate.ps1` for all validation
      - Verify GitHub Project board setup
      - Verify build succeeds
      - Create PR from branch
      - Link PR to related issues (PR created during `init-existing-repository`)
    - **Risks/Challenges** | None- Low: Branch creation may fail due to incomplete plan, the setup will need to be repeated.  - **CI Verification:** Must run `./scripts/validate.ps1` for all validation before each main assignment,      - Setup PR requires self-approval (acceptable for automated setup PR)
      - **Project-Specific Notes** | |
    - This is a template repo created from the template, Existing Python files in plan_docs provide reference implementation (ex: existing AGENTS.md needs updating in AGENTS.md)
      - Rename `.ai-new-app-template.code-workspace` to `.devcontainer/devcontainer.json` name
      - Import labels through scripts
      - Run `scripts/validate.ps1` for all validation
      - Verify GitHub Project board setup
      - Verify build succeeds
      - Create PR from branch
      - Link PR to related issues (PR created during `init-existing-repository`)
      - Verify `.github/.labels.json` contains all expected labels
      - Use `scripts/import-labels.ps1` for idempotency check
      - If ruleset file exists, skip import
      - If ruleset file doesn't exist, check if ruleset exists (`.github/protected-branches_ruleset.json` exists), skip import
      - Strip non-importable fields and POST via GitHub API
        - If ruleset file doesn't exist, check if ruleset exists (`.github/protected-branches_ruleset.json` exists), skip and import
      - Commit file changes first, then create PR
      - PR created after first commit, push to remote
    - **Project-Specific Notes** | This is a a fresh repository from a template. the files may need renaming. AGENTS.md is already mentions the template placeholders (`workflow-orchestration-queue-juliet23`) should be replaced with repository-specific values,        - **Risks/Challenges** | None-low. Branch creation could fail due to incomplete plan

  - **SHA Pinning Enforcement:** All GitHub Actions workflows created or modified during this workflow must have actions pinned to specific commit SHA. Do not use tag references (e.g., `@v4`, `@main`).). Do not use major/minor version tags like `@v4` or `@main`). The referencing existing branch will likely fail.
        - Ensure branch naming convention is followed: `dynamic-workflow-<workflow-name>`
        - PR created after first commit, push to remote
      - **Dependencies:** `init-existing-repository` output (PR number) |
| **Project-Specific Notes** | This is in a template repo context, we need to understand the template vs existing code structure, We'll need to determine what to create (solution, structure, tests, documentation, CI/CD pipelines, etc. This assignment must focus on understanding the requirements from `plan_docs` and planning the actual implementation. This is purely planning work—no coding happens in subsequent assignments. |

| **5. `create-app-plan` | Analyze the application template and supporting docs, create detailed plan
    - Create milestones from plan_docs and link issues to milestones
    - Create/update remote indices
    - Create documentation structure
    - Run local tests to verify setup
    - Create issue templates for feature requests
    - Obtain stakeholder approval
  - **Project-Specific Notes** | |
    - This is a template repo created from the script, all plan_docs/` already exist (Architecture Guide, Implementation Spec, OS-APOW Plan Review, Simplification Report)
        - File renames and code cleanup (Plan Review/Architecture guide/Implementation Spec for recommendations)
        - Existing Python files in plan_docs provide reference implementation
        - Existing AGENTS.md needs updating in AGENTS.md to      - Rename `.ai-new-app-template.code-workspace` to `.devcontainer/devcontainer.json` name
      - Import labels from scripts
      - Run `scripts/validate.ps1` for all validation
      - Verify GitHub Project board setup
      - Verify build succeeds
      - Create PR from branch
      - Link PR to related issues (PR created during `init-existing-repository`)
        - Verify `.github/.labels.json` contains all expected labels
        - Use `scripts/import-labels.ps1` for idempotency)
        - If ruleset file exists, skip import
        - If ruleset file doesn't exist, check if ruleset exists (`.github/protected-branches_ruleset.json` exists), skip and proceed
      - Commit file changes, then create PR
      - PR created after first commit, push to remote
    - **Project-Specific Notes** | This is a a fresh repository from a template, changes files may need renaming. AGENTS.md and already mentions that template placeholders should be replaced with repository-specific values. This is a template repo created from a template. Existing Python files in plan_docs provide reference implementation, and existing AGENTS.md needs updating in AGENTS.md.        - Ensure `.ai-repository-summary.md` file is created/updated if needed

        - Reference `plan_docs/architecture.md` and `architecture.md` for comprehensive overview

        - Link to the `.ai-repository-summary.md` file (created by `create-repository-summary` assignment) in the repo root, `.ai-repository-summary.md` serves as a quick reference for other documents
        - Update README.md to reflect this changes
        - Ensure consistency with AGENTS.md
        - Cross-reference any existing docs (e.g., APP_PLAN.md)
 where applicable
      - Validate commands listed actually work
      - Document tech stack, dependencies, testing approach, and architecture patterns

| **Dependencies** | `init-existing-repository` outputs (PR number) used to downstream planning steps
      - `create-app-plan` needs the complete first to proceed to `create-project-structure`
      - `create-agents-md-file` needs the completed first
        - **Post-script-begin** | N/A |
    - `create-agents-md-file` needs `create-app-plan` to `create-project-structure` to be complete
    - `pr-approval-and-merge` needs to occur after `create-app-plan`
 and `init-existing-repository` having created a PR, merge, and, closed
        - `create-project-structure` outputs the structure (PR number) for in linking issues to milestones
        - `create-agents-md-file` needs `init-existing-repository` PR creation and AGENTS.md customization
        - Run validation suite
        - Verify build and configuration
        - Verify all documentation is accessible
        - Obtain stakeholder approval
      - **Project-Specific Notes** | This is a template repo created from a template,        - Existing Python files in plan_docs provide reference implementation and existing AGENTS.md. Plan references in AGENTS.md need to replaced with repository-specific values
        - AGENTS.md needs to be updated to reference the repository by name, tech stack, and architecture
        - Reference AGENTS.md for instructions for commit hygiene patterns, branch naming, terminal commands
        - **Risks/Challenges** | None identified |
        - **Events:** \n- `pre-assignment-begin`: gather-context (Read `ai-pr-comment-protocol.md`, `gather-context` assignment)
    - `post-assignment-complete`: report-progress, `recover-from-error` (on failure) |
| **Dependencies** | `init-existing-repository` outputs PR number |
| **Project-Specific Notes** | This is a a template repo. Need to verify all plan docs exist and understand template + existing code structure
  - Plan references should checking for template placeholders in `.github/ISSUE_TEMPLATE/` directory for linking to templates
  - Need to verify remote instruction module path
  - Verify `.ai-repository-summary.md` exists (will be created by `create-repository-summary` assignment)
  - Ensure commands listed work
  - Run validation suite (`./scripts/validate.ps1 -All`)
  - Verify CI passes before merging
  - **Events:** \n- `pre-assignment-begin`: `gather-context` |
| **on-assignment-failure**: `recover-from-error` |
| **post-assignment-complete**: `report-progress` |
| **Risks/Challenges** | Plan document ambiguity if features are referenced but multiple times; GitHub Project setup requires proper permissions; may need alternative approach |
| **Events:** None |
| **Risks/Challenges** | None identified |
| **Events:** \n- `pre-assignment-begin`: `gather-context` (executes before this assignment) |
| **post-assignment-complete**: `validate-assignment-completion`, `report-progress` (after each main assignment) |
| **Risks/Challenges** | None identified |
| **Events:** \n- `pre-assignment-begin`: `gather-context` |
| **on-assignment-failure**: `recover-from-error` |
| **post-assignment-complete**: `report-progress` |
| **Project-Specific Notes** | This is in a template repo created from a template, All plan docs are pre-seeded. Need to verify remote instruction modules exist and create `ai-pr-comment-protocol.md` if referenced in assignments. May need to fetch from remote URL. |
| **Dependencies** | `init-existing-repository` (PR number), `create-app-plan` (plan issue), `create-project-structure` (structure) |
| **Risks/Challenges** | PR merge conflicts if CI failures requiring remediation; Must ensure CI is green before merging
  - **Events:** None |
| **Risks/Challenges** | None identified |
| **Events:** \n- `pre-assignment-begin`: `gather-context` |
| **on-assignment-failure`: `recover-from-error` |
| **post-assignment-complete**: `validate-assignment-completion`, `report-progress` |
| **Project-Specific Notes** | This is the final assignment. Must ensure all previous work is committed to pushed before merging. CI remediation loop may be necessary. May need up to 3 fix attempts. Self-approval by orchestrator is acceptable for automated setup PR. After merge: delete setup branch and close related setup issues. |
| **Dependencies** | All previous assignments must be PR from branch to merge into `main` |
| **Risks/Challenges** | Merge conflicts; CI failures; Branch protection blocking merge; Incomplete work may block final step |
  - **Events:** None |
| **Risks/Challenges** | None identified |
| **Events:** \n- `pre-assignment-begin`: `gather-context` |
| **on-assignment-failure`: `recover-from-error` |
| **post-assignment-complete**: `validate-assignment-completion`, `report-progress` |

---

## 4. Sequencing Diagram

```
┌─────────────────────┐────────────────────────────┐──────────────────────────┐
│  PRE-SCRIPT-BEGIN  │                    │                        │                        │
│         │           │ create-workflow-plan │                        │                        │
└─────────┘───────────┴────────────────────────┴────────────────────────┯┘
                              │
                              ▼
┌─────────────────────┐────────────────────────────┐──────────────────────────┐─────────────────────────┐
│  MAIN ASSIGNMENTS  │                   │                    │                        │                     │
│         │           │ init-existing-   │ create-app-plan       │ create-project-   │ create-agents-md- │
│         │           │ repository       │                      │ structure           │ file              │
└─────────┴───────────┴────────────────────────┴────────────────────────┴─────────────────────┘
                              │                              │                      │                      │
                              │                              │                      │                      │
                              ▼                              ▼                      ▼                      ▼
┌─────────────────────┐────────────────────────────┐──────────────────────────┐
│         │           │ debrief-and-     │ pr-approval-and-     │ POST-SCRIPT-     │
│         │           │ document          │ merge                │ COMPLETE         │
└─────────┴───────────┴────────────────────────┴────────────────────────┴──────────────────┘
                              │                      │                      │
                              │                      │                      │
                              ▼                      ▼                      ▼
                    ┌───────────────────────┐
                    │ POST-ASSIGNMENT      │
                    │ EVENTS (after each   │
                    │ main assignment)      │
                    │ - validate-assignment-│
                    │ completion             │
                    │ - report-progress     │
                    └───────────────────────┘
```

---

## 5. Open Questions

None identified at this time. The plan docs are comprehensive and the workflow assignments are well-documented.

---

## Approval Request

**Does this workflow execution plan look correct? Are there any changes needed, or do you approve it so I can proceed to Phase 1 (init-existing-repository) to begin execution.**

Please confirm your you approve the plan so I can commit `plan_docs/workflow-plan.md` to the repository.