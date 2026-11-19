# Implement reliable task claiming and state transitions

## Objective

Build the PostgreSQL-backed scheduler and state machine for asynchronous workers. This is the first planned MVP implementation task.

## Design

- Define Pending, Claimed, Running, Succeeded and Failed states with explicit allowed transitions.
- Atomically claim pending tasks using a transaction and row locking (e.g. FOR UPDATE SKIP LOCKED).
- Maintain lease expiry, heartbeat, worker identity, and a monotonically increasing fencing token.
- Reject late writes from stale workers using a conditional update on the current fencing token and state.
- Reclaim expired leases; implement bounded retries and backoff.
- Persist task state and outbox event within the same transaction. Delivery is at least once; consumers must be idempotent.
- For external side effects, require downstream idempotency keys or fencing support; do not claim exactly-once execution without those guarantees.

## Acceptance criteria

- [ ] Concurrent workers cannot obtain the same active lease.
- [ ] Expired leases are reclaimed after simulated worker termination.
- [ ] A stale worker cannot commit a terminal state after reassignment.
- [ ] Repeated deliveries do not duplicate supported idempotent side effects.
- [ ] Outbox delivery resumes after a relay crash.
- [ ] Integration tests exercise a real PostgreSQL instance and deterministic race scenarios.
- [ ] Document retry semantics, failure modes, and local test instructions.

## Proposed agent assignments

- Implementation agent: scheduler and database migration.
- Infrastructure agent: Docker Compose and CI.
- Review agent: independent concurrency and recovery tests.

Coordinate database and API contracts before parallel changes. All work is subject to human review.
