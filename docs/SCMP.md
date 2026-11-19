# Software Configuration Management Plan (SCMP)

- **Project:** Inventory System Documentation & Process Simulation
- **Course:** SEN 401 – Software Configuration Management & Maintenance (Lecturer: Christian Oliver)
- **Version:** 0.1 (draft, baselined at tag `v0.2.0`)
- **Date:** 19 November 2026
- **Author:** Emmanuel (Configuration Manager)
- **Jira ticket:** SCM-5

## 1. Introduction

### 1.1 Purpose

This plan states, before the work starts, how configuration management is carried out on the
Inventory System project: who does what, with which tools, and to what standard. Every rule in it
is meant to be checkable by someone outside the team.

### 1.2 Objectives

- Identify every artefact the project depends on and place it under version control.
- Ensure no change reaches `main` without an approved Change Request (CR) and a CCB-approved Pull Request (PR).
- Record and report the status of every CI and CR weekly.
- Audit the delivered baseline against this plan and the requirements before each release.

### 1.3 Scope

- **Covers:** all files in the GitHub repository under the `team-e-sen` organisation, and all tickets
  in the Jira space `pau-sen-401` (key `SCM`).
- **Life-cycle phases:** requirements, architecture, test planning and release of documentation, from
  18 November 2026 to the end of the course.
- **Out of scope:** source code (this project has none) and personal scratch branches that are never
  opened as PRs.

### 1.4 Definitions

| Term | Meaning in this project |
| :--- | :--- |
| Configuration Item (CI) | Any file in the repository that the project depends on and needs to track (see Section 3.1) |
| Baseline | A reviewed, stable, reproducible snapshot of the CIs, marked with an annotated Git tag |
| Change Request (CR) | A Jira ticket proposing a change to a baselined CI, stating what, why and impact |
| CCB | Change Control Board: approves or rejects CRs and PRs |
| Software library | The GitHub repository: the single source of truth for every CI |

## 2. SCM Management

### 2.1 Organisational model

The team uses an **embedded (DevOps-style) model** with a small central SCM function: every member
works inside the agreed branching and PR workflow, while two Configuration Managers own the tooling,
baselines and reports. This suits a six-person team working on a short timeline.

### 2.2 Constraints

| Source | Constraint |
| :--- | :--- |
| External | SEN 401 course requirements and deadlines (Week 2: 20 Nov; Week 3: 27 Nov); IEEE 828 used as a reference structure |
| Internal | Tools are fixed by the course: Git, GitHub, Jira. GitHub Free plan means the repository must be public for branch protection to be enforced |
| Project | Six members, about two weeks, documentation-only deliverables |

### 2.3 Roles and responsibilities

| Role | Name(s) | Responsibilities |
| :--- | :--- | :--- |
| Configuration Manager (CM) | Ezinne, Emmanuel | Owns the SCM process. Maintains the repository, branch protection and Jira workflow. Merges approved PRs, tags baselines, keeps the baseline and surveillance logs, writes the weekly status report |
| Change Control Board (CCB) | Ezinne, Esuga, Emmanuel | Reviews every CR in Jira and every PR in GitHub. Approves, rejects or requests more information. Runs configuration audits |
| Development Team | Daniel, Gabriel, Treasure | Raises CRs. Creates feature branches. Writes the documentation. Opens PRs |
| Auditors (read-only) | Lecturer, TA | Inspect process compliance and audit trails in GitHub and Jira |

**Separation of duties:** no one approves their own CR or PR. A CCB member who raised or is assigned
to a CR leaves that decision to the other two members.

### 2.4 Access levels (least privilege)

| GitHub team / person | Repository access | Reason |
| :--- | :--- | :--- |
| `devs` | Write | Push feature branches and open PRs |
| `ccb` | Write | Approvals only count from users with at least Write |
| `config-managers` | Admin | Manage branch protection, settings and releases |
| Lecturer, TA | Read | Inspect evidence |

Organisation base permission is **Read**. Only the Configuration Managers are organisation Owners.

### 2.5 CCB meetings

The CCB meets **every Monday and Thursday at 20:00 WAT on Google Meet** for as long as the project runs.
Decisions are recorded in `docs/ccb_minutes.md` and as a comment on each Jira ticket. A CR waiting
more than 24 hours may be decided asynchronously by two CCB members in Jira comments.

### 2.6 Applicable policies and standards

- This SCMP
- `CONTRIBUTING.md`: branch naming and Definition of Done
- `.github/pull_request_template.md`: PR standard
- Definition of Done: CR approved, PR approved by a CCB member who is not the author, CI passing,
  no unresolved conversations, merged to `main`, Jira ticket in Done

## 3. SCM Activities

### 3.1 Configuration identification

The following are Configuration Items. Each is tracked in `docs/ci_register.md` with an ID, owner and
current version.

| CI-ID | File | Type | Owner |
| :--- | :--- | :--- | :--- |
| CI-01 | `docs/SCMP.md` | Plan | Emmanuel |
| CI-02 | `docs/requirements.md` | Requirements | Gabriel |
| CI-03 | `docs/architecture.md` | Design | Daniel |
| CI-04 | `docs/test_plan.md` | Test asset | Esuga |
| CI-05 | `CHANGELOG.md` | Release record | Config Managers |
| CI-06 | `README.md` | Documentation | Config Managers |
| CI-07 | `.github/CODEOWNERS` | Configuration | Config Managers |
| CI-08 | `.github/pull_request_template.md` | Configuration | Config Managers |
| CI-09 | `.github/workflows/` (markdownlint CI) | Pipeline definition | Treasure |

**Not CIs:** local editor settings, personal scratch branches, and anything generated by CI.

**Naming:** documentation files are lower-case with underscores in `docs/`. Baselines are tagged
`vMAJOR.MINOR.PATCH`. Branches follow Section 3.2.

**Baselines:**

| Tag | When | Contents |
| :--- | :--- | :--- |
| `v0.1.0` | After the repository setup CR is merged | Repository skeleton, PR template, CODEOWNERS |
| `v0.2.0` | 20 Nov 2026 | Week 2 documents, SCMP, CHANGELOG |
| `v1.0.0` | 27 Nov 2026 | Audited release after status accounting and audit |

A baseline is tagged only after its PRs are reviewed and merged, using
`git tag -a vX.Y.Z -m "<message>"` then `git push --tags`, and is recorded in `docs/baseline_log.md`.

### 3.2 Configuration control

No one pushes directly to `main`. Branch protection enforces this (Section 4). Every change follows these steps:

1. **Request:** the author raises a Jira CR stating the affected CI, the justification and the impact.
2. **CCB review:** the CCB asks four questions: Is the CI baselined? Is the reason specific? Is the impact
   known? Approve or reject? Vague CRs (e.g. "clean up code") are **Rejected** with a written reason.
3. **Branch:** only after the CR is Approved, the author branches from the latest `main` as
   `feature/<name>/<SCM-n>-<short-desc>`.
4. **Implement:** edit only the files the CR names. Commit messages start with the ticket key (`SCM-n: ...`).
5. **Pull request:** title starts with `SCM-n`, uses the PR template, and links the Jira ticket.
6. **Review:** a CCB member who is not the author approves. Markdown lint CI must pass and all
   conversations must be resolved.
7. **Merge:** a Configuration Manager merges with **Squash and merge** and deletes the branch.
   Exception: the CHANGELOG merge-conflict exercise uses **Create a merge commit** so the conflict
   resolution stays visible in history.
8. **Close:** the Jira ticket moves to Done.

**Merge conflicts** are resolved by pulling `main` into the branch and agreeing the combined content with
the other author. Force-pushing shared history is not allowed.

### 3.3 Configuration status accounting

- **Jira workflow:** To Do → CCB Review → Approved **or Rejected** → In Progress → In Review → Done. Only
  these transitions exist; "any status can move here" is disabled. Approve/Reject are restricted to the
  CCB; Done is restricted to the Configuration Managers.
- **Weekly report** (`docs/status_report_<week>.md`, every Friday, written by a Configuration Manager): open vs closed CRs,
  rejected CRs, merged PRs, CI pass rate, commits this cycle, and the current baseline tag. Figures
  are read from Jira's Development panel and the GitHub Insights tab.

### 3.4 Audits and reviews

- **Functional configuration audit:** each document in the baseline is checked against its CR and the
  requirements (e.g. every requirement in `requirements.md` has a matching test in `test_plan.md`).
- **Physical configuration audit:** the files in the tagged baseline are checked against
  `docs/ci_register.md`. No CI is missing, nothing outside the register was shipped, and the versions match.
- **When:** before tagging `v1.0.0`, using `docs/audit_checklist.md`. Performed by a CCB member who did
  not author the documents being audited.

### 3.5 Surveillance

A Configuration Manager checks weekly that the plan is being followed and records the results in
`docs/surveillance_log.md`.

| Control | Evidence | Owner | Cadence | Failure signal |
| :--- | :--- | :--- | :--- | :--- |
| Authorised changes | Every merged PR links an Approved Jira CR | Configuration Manager | Weekly | A merge with no approved CR |
| CI runs | GitHub Actions results on each PR | Configuration Manager | Every PR | Missing or failed checks merged anyway |
| Baseline integrity | Tag contents vs `ci_register.md` | Configuration Manager | Each release | Tag does not match the register |
| Process adherence | Sample of 3 recent PRs: branch name, reviewer, approval | CCB | Weekly | Direct pushes, self-approval, wrong branch names |

## 4. Tools, techniques and environment

| Tool | Use | Key settings |
| :--- | :--- | :--- |
| Git | Version control | Annotated tags for baselines |
| GitHub (org `team-e-sen`, public repo) | Software library, PRs, reviews, releases | `main` protected: PR required, 1 approval, Code Owners review, stale approvals dismissed, most recent push must be approved by someone else, conversations resolved, required status check, no bypass, no force pushes, no deletions |
| Jira (space `pau-sen-401`, key `SCM`) | Change Requests, CCB decisions, status accounting | Custom workflow (Section 3.3) |
| GitHub Actions + markdownlint | CI on every PR | Required status check on `main` |
| GitHub for Jira | Links branches, commits and PRs to tickets by key | Installed for this repository only |

## 5. Supplier and subcontractor control

Third-party components are brought under control as follows:

- The markdownlint GitHub Action is pinned to a specific version in the workflow file. Upgrades require a CR.
- The GitHub for Jira app is installed for this repository only, approved by an organisation Owner.
- GitHub and Jira are cloud services. Their plan limits (GitHub Free, Jira trial ending around
  26 Nov 2026) are recorded here. Any rule a plan stops enforcing is still checked through surveillance.

## 6. Records collection and retention

| Record | Where | Kept until |
| :--- | :--- | :--- |
| Baseline tags and GitHub Releases | GitHub | Never deleted |
| PRs, reviews and comments | GitHub | End of course plus one semester |
| CRs and CCB decisions | Jira and `docs/ccb_minutes.md` | End of course plus one semester |
| Baseline, surveillance and status logs | `docs/` | Versioned with the repository |
| Evidence screenshots (branch protection, workflow tests) | Shared team folder | End of course plus one semester |

This SCMP is a living document. Any change to it goes through the same CR process as any other CI.
