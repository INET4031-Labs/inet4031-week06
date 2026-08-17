# Week 6: GitHub Actions and CI/CD

**Sprint 3 Async | Due before Sprint 3 Review**

## Overview

In this week, your team builds two GitHub Actions workflows: a CI pipeline that builds and pushes your Flask Docker image on every pull request and push to main, and a scheduled maintenance workflow that runs on a weekly cron schedule. You will configure workflow secrets for credentials, enforce branch protection rules that require CI to pass before merging, and observe how a failing pipeline blocks a merge. By the end of this week, you will have a working CI/CD pipeline with automated image building and a scheduled maintenance workflow.

## Learning Objectives

- Write a GitHub Actions workflow for Docker build and image push to GitHub Container Registry
- Configure workflow secrets to securely pass credentials without hardcoding them in YAML
- Enforce branch protection so CI must pass before a PR can be merged
- Write a scheduled workflow using cron syntax (GitHub Actions timing, not systemd timers)
- Trigger a pipeline failure deliberately and observe the merge block in action

## Prerequisites

- Week 5 complete: monitoring stack deployed and verified
- GitHub repository with Write access for all team members
- Access to GitHub Container Registry (ghcr.io) through your GitHub account

## Pulling This Week's Starter Content Into Your Team Repo

This repo (`inet4031-week06`) is instructor-provided starter/reference content for
Week 6, not something you clone standalone. Pull the pieces you need into your
team's single repo:

```bash
git remote add week6 https://github.com/INET4031-Labs/inet4031-week06.git
git fetch week6
git checkout week6/main -- .github/workflows docs
git remote remove week6
```

Do this before you start editing `.github/workflows/` locally, or your local changes
will be silently overwritten by the checkout. Note: this week does not yet ship a
`scripts/check-week6.sh` -- none exists to pull.

## Architecture Assumption

This week uses GitHub Actions, which runs on GitHub's infrastructure outside your team container. The scheduling uses GitHub Actions cron syntax, not systemd timers inside the container. This is intentional: systemd timer behavior inside nested Docker containers is unreliable. GitHub Actions ensures the workflow runs on a predictable schedule regardless of your container state.

## Team Role Distribution

No single role should complete this week solo. Work assignments:

- **System Admin:** leads Part 1 (CI pipeline workflow creation) and Part 3 (scheduled workflow setup)
- **Developers:** implement workflow YAML, test branch protection, create test failure
- **QA:** verifies all validation checks pass, confirms merge block behavior
- **Scrum Master:** keeps sprint board updated, coordinates hand-offs between parts

## TODOs for This Week

Complete the following parts in order. Each part builds on the previous one.

### Part 1: CI Pipeline for Docker Build and Push

TODO: Create `.github/workflows/ci.yml` workflow file
- Trigger on: push to main, pull requests to main, manual workflow_dispatch
- Environment variables: REGISTRY=ghcr.io, IMAGE_NAME=${{ github.repository }}/flask-app
- Steps:
  - Checkout code
  - Log in to GitHub Container Registry using GITHUB_TOKEN (automatic, no PAT needed)
  - Extract metadata and tags (sha, branch, latest on main)
  - Build and push Docker image using docker/build-push-action
  - Push only happens on push/merge to main, NOT on pull requests
- Commit the workflow file

See Part 1 in the lab directions for exact workflow YAML.

### Part 2: Workflow Secrets and Branch Protection

TODO: Create a workflow secret in GitHub
- Navigate to Settings > Secrets and variables > Actions > New repository secret
- Name: `SLACK_WEBHOOK_URL`
- Value: (placeholder for now, or leave empty)
- Save the secret

TODO: Configure branch protection for main branch
- Navigate to Settings > Branches > Add rule
- Branch name pattern: main
- Enable "Require a pull request before merging"
- Enable "Require status checks to pass before merging"
- Add the build-and-push status check
- Enable "Require branches to be up to date before merging"
- Save the rule

TODO: Create a test branch and intentional build failure
- Create branch: test/failing-build
- Modify Dockerfile: change FROM python:3.11-slim to FRMM python:3.11-slim (invalid syntax)
- Commit and push to test/failing-build
- Open a pull request from test/failing-build to main
- Wait for CI to run and verify the merge button is BLOCKED
- Take screenshot of the blocked merge
- Fix the Dockerfile (restore valid syntax)
- Push the fix and watch CI re-run
- After CI passes, merge the PR and delete the test branch
- Take screenshot of successful CI allowing the merge

### Part 3: Scheduled Maintenance Workflow

TODO: Create `.github/workflows/scheduled-maintenance.yml`
- Trigger: cron schedule at 08:00 UTC every Monday (0 8 * * 1)
- Include workflow_dispatch for manual triggering
- Jobs:
  - weekly-report job: runs on ubuntu-latest
  - Steps:
    - Checkout code
    - Report workflow run (echo statements showing date and repository)
    - Scan image for critical vulnerabilities using Trivy (aquasecurity/trivy-action)
- Commit the workflow file

TODO: Manually trigger the scheduled workflow from Actions tab
- Navigate to Actions tab
- Select "Scheduled Maintenance" workflow
- Click "Run workflow" and confirm
- Wait for completion and verify it succeeded

## Storage Check

Run these commands and record output in your team's Google Doc under "Week 6 Storage Check":

```bash
df -h
docker system df
```

Compare to your Week 5 baseline. GitHub Actions workflows do not consume local disk space; they run on GitHub's infrastructure.

## Validation Checks

All validation checks must pass before marking deliverables complete.

**Validation Check: CI Workflow Passes on main**
- Navigate to Actions tab in your GitHub repository
- The most recent run on main branch shows a green checkmark
- Workflow completed successfully

**Validation Check: Branch Protection Configured**
- Navigate to Settings > Branches
- Confirm the main rule requires status checks to pass before merge

**Validation Check: Scheduled Workflow Runs Successfully**
- In the Actions tab, find "Scheduled Maintenance" workflow
- Confirm it completed without error when manually triggered
- Look for green checkmark on the run

**Validation Check: Check Script Passes**
- Run from your team container: `./scripts/check-week6.sh`
- Expected: all checks pass

## Deliverables

Before marking this week complete, confirm you have:

- `.github/workflows/ci.yml` committed to your repository
- `.github/workflows/scheduled-maintenance.yml` committed to your repository
- Branch protection rule configured on main branch (verified in Settings)
- Screenshot of PR blocked from merging (failed CI check visible)
- Screenshot of PR unblocked after CI passes (green checkmark visible)
- `./scripts/check-week6.sh` runs clean in your team container
- Google Doc updated with Week 6 reflections and storage check

## Screenshots Required for Google Doc

Add these screenshots to your team Google Doc under "Week 6 Screenshots":

- **Screenshot 1:** Pull request blocked from merging with failing CI check (red X visible)
- **Screenshot 2:** Pull request unblocked after CI passes (green checkmark visible)
- **Screenshot 3:** Scheduled Maintenance workflow run showing success
- **Screenshot 4:** `./scripts/check-week6.sh` passing

## Discussion Questions

Answer these questions in your team Google Doc under "Week 6 Reflections":

1. Your CI runs on both push to main and on pull requests. What is the difference between these two triggers in terms of when your team gets feedback about broken builds?

2. The scheduled workflow uses cron syntax. Why did this lab choose GitHub Actions instead of a systemd timer running inside your team container?

3. Secrets stored in GitHub Actions are never printed in logs. Is this protection foolproof? What else would a security-conscious team do in addition to GitHub's secret masking?

4. Branch protection required CI to pass before merging. Name one scenario where a team might need to bypass branch protection, and describe what governance controls a larger organization would put in place to prevent abuse.

5. (Extension question) Your CI pipeline builds a Docker image but does not run automated tests on the application code itself. What would you add to make CI provide stronger quality guarantees before a push to main?

## Next Steps

After completing this week:
- Merge all outstanding PRs
- Update the sprint board: move completed items to Done
- Prepare for Sprint 3 Review (synchronous session)
