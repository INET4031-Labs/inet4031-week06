# Environment State Log

**Week 6 Async**

## Container Environment Snapshot

Record the state of your team container at key points during this week.

### Start of Week 6

Timestamp: TODO

Record the output of these commands at the beginning of the week:

```bash
docker ps
docker compose -f week-2/docker-compose.yml ps
k3d cluster list
kubectl get pods
git log --oneline -5
```

Output:

```
TODO: Paste start-of-week environment state here
```

### End of Week 6

Timestamp: TODO

Record the same commands after all work is complete:

```bash
docker ps
docker compose -f week-2/docker-compose.yml ps
k3d cluster list
kubectl get pods
git log --oneline -5
```

Output:

```
TODO: Paste end-of-week environment state here
```

## GitHub Actions Environment

Record information about your GitHub Actions workflows:

### CI Workflow Status

- Workflow file path: `.github/workflows/ci.yml`
- Created: TODO (date)
- First successful run: TODO (date)
- Latest run result: TODO (pass/fail)
- Latest run timestamp: TODO

### Scheduled Maintenance Workflow Status

- Workflow file path: `.github/workflows/scheduled-maintenance.yml`
- Created: TODO (date)
- First manual run: TODO (date)
- Latest run result: TODO (pass/fail)

## Branch Protection Status

- Branch: main
- Protection enabled: TODO (yes/no)
- Status checks required: TODO (ci check name)
- Date configured: TODO

## Storage Usage

Record output from storage check commands:

Filesystem usage:
```
TODO: Paste df -h output here
```

Docker storage:
```
TODO: Paste docker system df output here
```

Comparison to Week 5 baseline:
- Filesystem used: TODO (compare to Week 5)
- Docker images size: TODO (compare to Week 5)
- Docker containers: TODO (compare to Week 5)
- Docker volumes: TODO (compare to Week 5)

## Issues or Unexpected Behavior

Note any environment issues encountered:

- TODO: Record any issues and resolution

## Assumptions Made

Record any assumptions your team made about the Week 6 environment:

- TODO: Add assumptions here
