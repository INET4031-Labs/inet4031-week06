## Week 6: GitHub Actions and CI/CD

**Sprint 3 Async | Due before Sprint 3 Review**

### Overview

In this lab, your team builds two GitHub Actions workflows: a CI pipeline that builds and pushes your Flask Docker image on every pull request, and a scheduled maintenance workflow that runs weekly. You will configure workflow secrets, enforce a branch protection rule that requires CI to pass before merging, and observe how a failing pipeline blocks a merge. After completing this lab, you will have a working CI/CD pipeline committed to your repository that validates every code change automatically and a scheduled workflow running on a defined cron schedule.

> **Scheduling note:** This lab uses GitHub Actions cron syntax for scheduling, not systemd timers. Systemd timer behavior inside nested Docker containers is unreliable. GitHub Actions runs on GitHub's infrastructure outside the container, making it the correct tool for scheduled workflows in this environment.

### Learning Objectives

- Write a GitHub Actions workflow for Docker build and image push
- Configure workflow secrets for credentials without hardcoding them in YAML
- Enforce branch protection so CI must pass before a PR can merge
- Write a scheduled workflow using cron syntax
- Trigger a pipeline failure deliberately and observe the merge block

### Prerequisites

- Week 5 complete: monitoring stack deployed
- GitHub repository with Write access for all team members
- Access to GitHub Container Registry (ghcr.io)

---

### Part 1: CI Pipeline for Docker Build and Push

**Step 1.** Create the GitHub Actions workflow directory inside your repo.

```bash
mkdir -p .github/workflows
```

**Step 2.** Create `.github/workflows/ci.yml`.

```yaml
name: CI -- Build and Push Docker Image

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  workflow_dispatch:

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}/flask-app

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Log in to GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Extract metadata for Docker
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=sha,prefix=sha-
            type=ref,event=branch
            type=raw,value=latest,enable=${{ github.ref == 'refs/heads/main' }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: ./week-2/app
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

> **Note on authentication:** This workflow uses `GITHUB_TOKEN`, automatically provided by GitHub Actions. No team member needs to create or store a personal access token for pushes to ghcr.io. `GITHUB_TOKEN` is scoped to the current repository and expires when the workflow finishes.

**Step 3.** Commit and push the workflow.

```bash
git add .github/
git commit -m "feat: add CI workflow for Docker build and push"
git push origin main
```

**Step 4.** Navigate to the Actions tab on your GitHub repository. Wait for the workflow run triggered by your push to complete. If it fails, check the error output and fix.

**Discussion (add to Google Doc):** The workflow triggers on both push to main and on pull requests. What is the difference between these two triggers in terms of when your team gets feedback?

---

### Part 2: Workflow Secrets and Branch Protection

**Step 5.** Navigate to Settings > Secrets and variables > Actions > New repository secret. Create a secret named `SLACK_WEBHOOK_URL` with a placeholder value (for use in Part 3).

> **Enterprise Pattern:** Workflow secrets are injected at runtime by GitHub. They are never printed in logs, inaccessible to forked repositories by default, and encrypted at rest.

**Step 6.** Configure branch protection for `main`. Navigate to Settings > Branches > Add rule.

- Branch name pattern: `main`
- Enable: "Require a pull request before merging"
- Enable: "Require status checks to pass before merging"
- Add the `build-and-push` status check
- Enable: "Require branches to be up to date before merging"
- Save the rule.

**Step 7.** Create a new branch and introduce a build failure.

```bash
git checkout -b test/failing-build
```

Open `week-2/app/Dockerfile` and change `FROM python:3.11-slim` to `FRMM python:3.11-slim` (intentional syntax error).

```bash
git add week-2/app/Dockerfile
git commit -m "test: intentional build failure to verify branch protection"
git push origin test/failing-build
```

**Step 8.** Open a pull request from `test/failing-build` to `main` on GitHub. Wait for CI to run. Observe that the merge button is disabled and blocked after the build fails.

**Take a screenshot** of the pull request showing the blocked merge and failed check. Add it to your Google Doc.

**Step 9.** Fix the Dockerfile, push the fix, and watch CI re-run.

```bash
git add week-2/app/Dockerfile
git commit -m "fix: restore valid Dockerfile FROM instruction"
git push origin test/failing-build
```

After CI passes, merge the PR on GitHub. Delete the test branch.

---

### Part 3: Scheduled Maintenance Workflow

**Step 10.** Create `.github/workflows/scheduled-maintenance.yml`.

```yaml
name: Scheduled Maintenance

on:
  schedule:
    - cron: '0 8 * * 1'
  workflow_dispatch:

jobs:
  weekly-report:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Report workflow run
        run: |
          echo "Weekly maintenance completed at $(date -u)"
          echo "Repository: ${{ github.repository }}"
          echo "Triggered by: ${{ github.event_name }}"

      - name: Scan image for critical vulnerabilities
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: 'ghcr.io/${{ github.repository }}/flask-app:latest'
          format: 'table'
          exit-code: '0'
          severity: 'CRITICAL'
```

> **Cron syntax:** `0 8 * * 1` means "08:00 UTC every Monday." Five fields: minute, hour, day of month, month, day of week (1 = Monday).

**Step 11.** Trigger the workflow manually from the Actions tab.

**Step 12.** Commit the scheduled workflow.

```bash
git add .github/
git commit -m "feat: add weekly scheduled maintenance workflow"
git push origin main
```

---

### Storage Check

```bash
df -h
docker system df
```

Record in your Google Doc under "Week 6 Storage Check."

---

### Validation Checks

#### Validation Check: CI Workflow Passes on main

Navigate to Actions tab. The most recent run on `main` shows a green checkmark.

#### Validation Check: Branch Protection Configured

Navigate to Settings > Branches. Confirm the `main` rule requires status checks.

#### Validation Check: Scheduled Workflow Runs Successfully

In the Actions tab, confirm the Scheduled Maintenance workflow completed without error when manually triggered.

#### Validation Check: Check Script Passes

```bash
./scripts/check-week6.sh
```

---

### Deliverables

- `.github/workflows/ci.yml` committed (build and push, manual trigger)
- `.github/workflows/scheduled-maintenance.yml` committed
- Branch protection rule configured on `main`
- Screenshot of failed CI blocking a merge
- Screenshot of passing CI after fix
- `./scripts/check-week6.sh` runs clean

**Screenshot requirements:**

- **Screenshot 1:** PR blocked from merging with failing CI check
- **Screenshot 2:** PR unblocked after CI passes
- **Screenshot 3:** Scheduled Maintenance workflow run success
- **Screenshot 4:** `./scripts/check-week6.sh` passing

---

### Reflection Questions (Answer in Google Doc)

1. Your CI runs on both push to main and on PRs. What would happen to your team's workflow if CI only ran on pushes to main? What is the cost of finding a broken build after merge?
2. The scheduled workflow uses cron syntax. Why did this lab choose GitHub Actions instead of a systemd timer inside the container?
3. Secrets stored in GitHub are never printed in logs. Is this foolproof? What would a security-conscious team do in addition to relying on GitHub's secret masking?
4. Branch protection required CI to pass before merging. Name one case where a team might bypass branch protection, and describe what governance controls a larger organization would put in place.
5. (Extend) Your CI pipeline builds a Docker image but does not run automated tests on the application code. What would you add to make CI provide stronger guarantees before a push to main?

---

---

