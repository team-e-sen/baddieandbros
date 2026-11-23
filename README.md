# Inventory Management System - SCM Project

## Project Overview

This repository contains the Software Configuration Management (SCM) documentation and process simulation for an Inventory Management System.

**Note:** This project is a process-simulation exercise. We are applying enterprise-level SCM practices (Jira, GitHub, Pull Requests, CCB approvals, and CI/CD) to manage documentation artifacts (Requirements, Architecture, Test Plans, and SCM Plans) rather than writing application code.

## The Team (Group E)

| Name | Matric Number | Role | GitHub Username |
| :--- | :--- | :--- | :--- |
| Queen Esther Emeanua | 23120112016 | Config Manager & CCB | @qemeanua |
| Emmanuel Abbah | 23120112001 | Config Manager & CCB | @e-abbah |
| Paul Majiyebo Esuga | 24120112061 | CCB & QA | @Paul-Esuga |
| Daniel Ezinna | 23120112021 | Developer / Documenter | @daniel-ezinna |
| Gabriel Mamman | 23120112028 | Developer / Documenter | @gmghorse |
| Treasure Onwuchekwa | 23120112046 | Developer / DevOps | @tocjpapi |

## Tools Used

* **Version Control:** GitHub
* **Issue Tracking:** Jira (Integrated with GitHub)
* **CI/CD:** GitHub Actions

## Repository Structure

* `.github/` - Contains CODEOWNERS and Pull Request templates.
* `docs/` - Contains all SCM documentation (SCMP, Requirements, Architecture, Test Plan).
* `CHANGELOG.md` - Tracks the release history and baselines.

## SCM Workflow (How we work)

1. **Change Request:** Every task starts as a ticket in Jira.
2. **Branching:** Developers create a branch using the format `feature/<username>/<Jira-ID>-<description>`.
3. **Pull Requests:** No direct pushes to `main`. Developers open PRs.
4. **CCB Approval:** The Change Control Board (Ezinne, Emmanuel, Esuga) must review and approve every PR.
5. **Merge:** Once CI passes and CCB approves, the branch is merged into `main` and tagged with a release baseline.
