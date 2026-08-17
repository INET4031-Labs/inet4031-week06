# Week 6 Acceptance Criteria

**GitHub Actions CI/CD Implementation**

## Overview

This document defines the acceptance criteria for all Week 6 deliverables. Every criterion must be met for the week to be marked complete. QA is responsible for verifying each criterion.

## Part 1: CI Pipeline for Docker Build and Push

### Criterion 1.1: Workflow File Exists and Is Committed

**Requirement:** `.github/workflows/ci.yml` exists in the repository

**How to verify:**
```bash
git show HEAD:.github/workflows/ci.yml
```

**Expected result:** Valid YAML workflow file is displayed without errors

**Acceptance:** TODO (pass/fail/notes)

### Criterion 1.2: Workflow Triggers on Push and Pull Request

**Requirement:** Workflow is configured to run on:
- Push to main branch
- Pull requests to main branch
- Manual workflow_dispatch

**How to verify:** Examine the `on:` section in `.github/workflows/ci.yml`

**Expected result:** Workflow file shows all three triggers configured

**Acceptance:** TODO (pass/fail/notes)

### Criterion 1.3: Docker Image Is Built and Pushed

**Requirement:** Workflow includes steps to:
- Log in to GitHub Container Registry
- Build Docker image from week-2/app context
- Push image to ghcr.io with appropriate tags

**How to verify:** Examine the workflow steps for docker/build-push-action

**Expected result:** Push step is conditional (only on main, not on PRs)

**Acceptance:** TODO (pass/fail/notes)

### Criterion 1.4: First CI Run Succeeds

**Requirement:** After pushing `.github/workflows/ci.yml` to main, the workflow runs and completes successfully

**How to verify:** Check Actions tab in GitHub repository

**Expected result:** Green checkmark on most recent main branch run

**Acceptance:** TODO (pass/fail/notes)

## Part 2: Workflow Secrets and Branch Protection

### Criterion 2.1: Workflow Secret Is Created

**Requirement:** A repository secret named `SLACK_WEBHOOK_URL` exists in GitHub Actions secrets

**How to verify:** Navigate to Settings > Secrets and variables > Actions

**Expected result:** Secret appears in the list (value masked)

**Acceptance:** TODO (pass/fail/notes)

### Criterion 2.2: Branch Protection Is Configured

**Requirement:** The main branch has a protection rule requiring:
- Pull request before merge
- Status checks must pass (build-and-push job)
- Branches must be up to date before merging

**How to verify:** Navigate to Settings > Branches and inspect the main rule

**Expected result:** All three requirements are enabled and visible in the rule

**Acceptance:** TODO (pass/fail/notes)

### Criterion 2.3: Failed CI Blocks Merge

**Requirement:** A pull request with failing CI cannot be merged, merge button is disabled

**How to verify:**
1. Create test/failing-build branch
2. Introduce build failure (invalid Dockerfile)
3. Push and open PR to main
4. Observe that merge button is disabled with message about status check

**Expected result:** PR shows red X on build-and-push check, merge button is disabled

**Acceptance:** TODO (pass/fail/notes)

**Screenshot attached:** TODO (yes/no)

### Criterion 2.4: Passing CI Unblocks Merge

**Requirement:** After fixing the build failure, CI passes and merge button becomes enabled

**How to verify:**
1. Fix the Dockerfile on test/failing-build branch
2. Push the fix
3. Wait for CI to re-run
4. Observe merge button becomes enabled

**Expected result:** PR shows green checkmark on build-and-push check, merge button is enabled

**Acceptance:** TODO (pass/fail/notes)

**Screenshot attached:** TODO (yes/no)

## Part 3: Scheduled Maintenance Workflow

### Criterion 3.1: Scheduled Workflow File Exists and Is Committed

**Requirement:** `.github/workflows/scheduled-maintenance.yml` exists in the repository

**How to verify:**
```bash
git show HEAD:.github/workflows/scheduled-maintenance.yml
```

**Expected result:** Valid YAML workflow file is displayed without errors

**Acceptance:** TODO (pass/fail/notes)

### Criterion 3.2: Workflow Uses Cron Syntax

**Requirement:** Workflow is configured to run on schedule: 0 8 * * 1 (08:00 UTC every Monday)

**How to verify:** Examine the `schedule:` section in the workflow

**Expected result:** Cron expression is exactly `0 8 * * 1`

**Acceptance:** TODO (pass/fail/notes)

### Criterion 3.3: Workflow Includes Trivy Scan

**Requirement:** Scheduled workflow includes a step to scan the Flask image for critical vulnerabilities using Trivy

**How to verify:** Examine the steps in the weekly-report job

**Expected result:** aquasecurity/trivy-action is used to scan ghcr.io image

**Acceptance:** TODO (pass/fail/notes)

### Criterion 3.4: Scheduled Workflow Can Run Manually

**Requirement:** Workflow can be triggered manually via `workflow_dispatch` and completes successfully

**How to verify:**
1. Navigate to Actions tab
2. Select "Scheduled Maintenance" workflow
3. Click "Run workflow"
4. Wait for completion

**Expected result:** Workflow completes with green checkmark

**Acceptance:** TODO (pass/fail/notes)

**Screenshot attached:** TODO (yes/no)

## Validation Checks

### Validation Check: All Scripts Pass

**Requirement:** `./scripts/check-week6.sh` completes without errors

**How to verify:** Run in team container:
```bash
./scripts/check-week6.sh
```

**Expected result:** Script exits with status 0 (success)

**Acceptance:** TODO (pass/fail/notes)

## Overall Week 6 Sign-Off

**QA Reviewer:** TODO (name)

**Date:** TODO

**All criteria met:** TODO (yes/no)

**Notes or exceptions:** TODO (add any notes)
