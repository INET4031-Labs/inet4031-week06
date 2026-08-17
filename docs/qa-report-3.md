# QA Report: Week 6

**Sprint 3 Async | GitHub Actions and CI/CD**

## QA Overview

This report documents quality assurance verification for Week 6. The QA role is responsible for running all validation checks, verifying that acceptance criteria are met, and signing off before deliverables are marked complete.

## QA Reviewer Information

**QA Team Member:** TODO (name)

**Week 6 Start Date:** TODO

**Week 6 Completion Date:** TODO

**Total time spent on QA:** TODO (hours)

## Pre-QA Checklist

Before starting validation checks, verify:

- [ ] All developers have finished their assigned work
- [ ] All files are committed to the main branch
- [ ] The sprint board shows the week's work
- [ ] No merge conflicts are outstanding
- [ ] Team is ready for validation

## Validation Check Results

### Check 1: CI Workflow Passes on main

**Check description:** Navigate to Actions tab and verify most recent run on main branch shows green checkmark

**How to run:** GitHub web interface > Actions tab

**Expected result:** CI workflow completed successfully

**Result:** TODO (pass/fail)

**Details:** TODO (add details)

---

### Check 2: Branch Protection Configured

**Check description:** Verify main branch has protection rule requiring status checks to pass before merge

**How to run:** Settings > Branches > inspect main rule

**Expected result:** Rule exists and requires build-and-push status check

**Result:** TODO (pass/fail)

**Details:** TODO (add details)

---

### Check 3: Failed CI Blocks Merge

**Check description:** Verify that a PR with failing CI cannot be merged

**How to run:** Check git history for test/failing-build branch merge attempt

**Expected result:** Merge button was disabled when CI was failing

**Result:** TODO (pass/fail)

**Screenshots:** TODO (add screenshot reference)

**Details:** TODO (add details)

---

### Check 4: Scheduled Workflow Runs Successfully

**Check description:** Verify scheduled maintenance workflow completes without error when manually triggered

**How to run:** Actions tab > Scheduled Maintenance > Run workflow

**Expected result:** Workflow shows green checkmark on completion

**Result:** TODO (pass/fail)

**Screenshots:** TODO (add screenshot reference)

**Details:** TODO (add details)

---

### Check 5: Check Script Passes

**Check description:** Run ./scripts/check-week6.sh and verify all checks pass

**How to run:** From team container:
```bash
./scripts/check-week6.sh
```

**Expected result:** Script exits with status 0

**Result:** TODO (pass/fail)

**Output:**
```
TODO: Paste script output here
```

**Details:** TODO (add details)

---

## Deliverable Verification

### Deliverable 1: .github/workflows/ci.yml

**Status:** TODO (present/missing)

**Verification:**
- File exists: TODO (yes/no)
- Syntax valid: TODO (yes/no)
- Triggers correct: TODO (yes/no)
- Push step conditional: TODO (yes/no)

**Notes:** TODO

---

### Deliverable 2: .github/workflows/scheduled-maintenance.yml

**Status:** TODO (present/missing)

**Verification:**
- File exists: TODO (yes/no)
- Syntax valid: TODO (yes/no)
- Cron schedule correct: TODO (yes/no)
- Trivy scan included: TODO (yes/no)

**Notes:** TODO

---

### Deliverable 3: Branch Protection Rule

**Status:** TODO (configured/not configured)

**Verification:**
- Rule exists on main: TODO (yes/no)
- Requires status checks: TODO (yes/no)
- Requires PR before merge: TODO (yes/no)
- Requires branches up to date: TODO (yes/no)

**Notes:** TODO

---

### Deliverable 4: Screenshots in Google Doc

**Status:** TODO (present/missing)

**Verification:**
- Screenshot 1 (PR blocked): TODO (present/missing)
- Screenshot 2 (PR unblocked): TODO (present/missing)
- Screenshot 3 (scheduled workflow success): TODO (present/missing)
- Screenshot 4 (check script passing): TODO (present/missing)

**Notes:** TODO

---

## Issues Found and Resolution

List any issues discovered during QA. For each issue, record the resolution.

### Issue 1

**Description:** TODO (describe issue or write "No issues found")

**Severity:** TODO (blocker/major/minor)

**Resolution:** TODO (how was it fixed)

**Resolved:** TODO (yes/no)

---

### Issue 2

**Description:** TODO

**Severity:** TODO

**Resolution:** TODO

**Resolved:** TODO

---

## Cross-Week Dependencies Check

Verify that Week 6 work does not break earlier weeks:

**Week 5 monitoring stack still running:**
- Command: `kubectl get pods -n monitoring`
- Expected: All monitoring pods in Running state
- Result: TODO (pass/fail)
- Notes: TODO

**Week 4 application stack still running:**
- Command: `kubectl get pods`
- Expected: Flask, Nginx, Postgres pods Running
- Result: TODO (pass/fail)
- Notes: TODO

**Week 2 Docker Compose stack still running:**
- Command: `docker compose -f week-2/docker-compose.yml ps`
- Expected: All three services healthy
- Result: TODO (pass/fail)
- Notes: TODO

## Performance and Storage Impact

**Storage check performed:** TODO (yes/no)

**Filesystem usage change:** TODO (no significant change expected)

**Docker storage usage change:** TODO (no significant change expected)

**Notes:** TODO

## Google Doc Verification

Verify team's Google Doc has been updated with Week 6 entries:

**Week 6 Screenshots section:** TODO (complete/incomplete)

**Week 6 Reflections section:** TODO (complete/incomplete)

**Week 6 Storage Check section:** TODO (complete/incomplete)

**Notes:** TODO

## Overall QA Sign-Off

### Summary

All required verification checks completed. Document the overall quality of Week 6 deliverables.

**Quality assessment:** TODO (excellent/good/acceptable/needs work)

**Readiness for Sprint 3 Review:** TODO (ready/not ready)

**Blocking issues remaining:** TODO (none/list any blockers)

### QA Sign-Off

**QA Approved:** TODO (yes/no)

**Date signed off:** TODO

**Reviewer signature:** TODO (QA team member name)

**Scrum Master acknowledgment:** TODO (name and date)

## Notes and Recommendations

Record any observations about Week 6 that would help future sprints:

- TODO: Add recommendations or observations

---

## Appendix: Test Data and Scenarios

### Test Branch: test/failing-build

**Purpose:** Verify that branch protection blocks failed CI

**Branch status:** TODO (deleted/archived/checked out)

**PR outcome:** TODO (merged/closed)

**Screenshots taken:** TODO (yes/no)

---

## Reference: Acceptance Criteria Mapping

This QA report cross-references the acceptance criteria defined in `docs/acceptance-criteria.md`:

| Acceptance Criterion | Check Status | QA Verification | Notes |
|---|---|---|---|
| 1.1: Workflow file exists | TODO | TODO | TODO |
| 1.2: Triggers configured | TODO | TODO | TODO |
| 1.3: Docker build and push | TODO | TODO | TODO |
| 1.4: First run succeeds | TODO | TODO | TODO |
| 2.1: Secret created | TODO | TODO | TODO |
| 2.2: Branch protection | TODO | TODO | TODO |
| 2.3: Failed CI blocks merge | TODO | TODO | TODO |
| 2.4: Passing CI unblocks merge | TODO | TODO | TODO |
| 3.1: Scheduled workflow exists | TODO | TODO | TODO |
| 3.2: Cron schedule correct | TODO | TODO | TODO |
| 3.3: Trivy scan included | TODO | TODO | TODO |
| 3.4: Manual trigger works | TODO | TODO | TODO |
