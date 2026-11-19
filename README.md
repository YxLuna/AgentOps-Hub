# AgentOps Hub

A planned platform for managing personal infrastructure, API services, scheduled jobs, and multi-agent engineering workflows.

**Status:** MVP design and planning; implementation has not started.

## Architecture

- FastAPI: task and service management API
- React: operations dashboard
- PostgreSQL: authoritative task state, leases, and transactional outbox
- Redis: delivery notifications and optional worker queue; never the authoritative state store
- Docker Compose: local development infrastructure
- GitHub Actions: automated testing and integration checks

## Proposed repository layout

```text
apps/api/           # API service
apps/dashboard/     # Web UI
apps/worker/        # Asynchronous task executor
core/scheduler/     # Scheduling and lease management
core/state/         # State machine and concurrency control
core/agents/        # Agent adapters
core/events/       # Transactional outbox
infrastructure/    # Docker and database migrations
tests/              # Integration, concurrency and fault recovery tests
.github/workflows/  # CI configuration
```

## MVP roadmap

1. Build reliable task claiming, state transitions, and lease recovery.
2. Add transactional outbox and idempotent event handling.
3. Implement API endpoints and a minimal dashboard.
4. Add independent concurrency, recovery, and integration testing.

Each implementation task should use a separate branch and pull request. Human review and CI checks are required before integration. No production credentials should be committed.

See [first issue specification](docs/issue-001.md).
