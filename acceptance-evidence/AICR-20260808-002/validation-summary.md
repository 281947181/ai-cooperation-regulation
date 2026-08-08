# AICR-20260808-002 Acceptance Evidence

## Identity

```yaml
task_contract_id: AICR-20260808-002
task_contract_version: 1
task_contract_hash: bb498113a128228d8db3d1d60c091e9ffcdb732cd2384f54fb3ad934ec6076cb
reviewed_commit: aa002290cea99c7afdaeca41ffce5f9714867fd9
review_id: AICR-20260808-002/main/aa002290cea99c7afdaeca41ffce5f9714867fd9
reviewer_conclusion: ACCEPTED_WITH_LIMITATION
task_closure_status: PARTIAL
bridge_stop_status: INVALID_RESPONSE
limitation: T-15 has no verified browser delivery and valid structured Result round trip.
```

The implementation and offline verification are complete for integration
testing. The Task Contract itself is not closed: section 16 requires AC-16 and
section 17 permits only a partial or Bridge-failure result while T-15 remains
incomplete. The human review's `ACCEPTED_WITH_LIMITATION` conclusion is recorded
without translating it into a protocol PASS. The current deterministic Bridge
row is `INVALID_RESPONSE` because no valid identity-matching
`[CODEX_ACCEPTANCE_RESULT]` envelope was received.

## Capability boundary

| Capability | Evidence status |
| --- | --- |
| Skill installation and current-session discovery | Observed; snapshot hashes recorded |
| Request and Result protocol parsing | Complete (offline tests) |
| SQLite state management | Complete (offline tests) |
| Digest-based idempotency and concurrency control | Complete (offline tests) |
| Crash, retry, and timeout recovery | Complete (offline tests) |
| Exact target binding/discovery | Not accepted as end-to-end evidence |
| Browser delivery receipt | Not accepted as end-to-end evidence |
| Structured Result recovery | Not accepted as end-to-end evidence |
| T-15 full browser round trip | Not passed |

## State machine

Normal path:

```text
PREPARED -> CLAIMED -> SENT -> DELIVERED -> RESULT_RECEIVED
                                                |-> PASS
                                                |-> REWORK
                                                `-> BLOCKED
```

Local stop states:

```text
DELIVERY_FAILED
RESPONSE_TIMEOUT
INVALID_RESPONSE
IDENTITY_MISMATCH
TARGET_NOT_FOUND
```

Key invariants:

1. `review_id` is the SQLite primary key and is bound to one Request digest.
2. A live `send-guard` owns the only send claim and holds the write guard through
   submission and rendered-message confirmation.
3. `SENT`, `DELIVERED`, and `RESPONSE_TIMEOUT` recover by inspecting the fixed
   conversation; they do not directly resend.
4. A delivery retry requires visible absence proof and the total attempt count
   cannot exceed two.
5. PASS, REWORK, and BLOCKED are accepted only from a strictly parsed,
   identity-matching Result envelope.

## Evidence index

- `changed-files.txt`: reviewed commit file list, diff stat, and reproduction commands.
- `test-result.txt`: test names, commands, results, Contract hash, and Git checks.
- `skill-tree.txt`: user-local Skill entry path and sanitized directory structure.
- `validation-summary.md`: task boundary, state machine, compatibility, and limitations.
- `review-agents.txt`: corrective evidence audit/review identities, findings, and resolution.

The full commit diff is reproducible with:

```bash
git diff 437838646e5a07fdc553139187ad448657aa2b0e aa002290cea99c7afdaeca41ffce5f9714867fd9
```

## Regulation compatibility

```yaml
compatibility_baseline: main@437838646e5a07fdc553139187ad448657aa2b0e
compatibility_result: main@aa002290cea99c7afdaeca41ffce5f9714867fd9
breaking_change: false
migration_required: false
```

The repository has no declared `2.x` compatibility version or release tag, so
claiming `2.x -> 2.x` would be unsupported. Commit identities are used as the
machine-verifiable compatibility baseline instead. The reviewed changes are
additive protocol/template changes and do not remove existing public fields.

## Deferred recommendations

The following are useful V2 work, but are not represented as completed here:

- explicit Contract schema-version and required-field validation;
- signed Result metadata such as `result_hash`, `generated_at`, and
  `validator_identity`;
- a higher-level lifecycle enum layered over the existing deterministic Bridge
  states, if consumers demonstrate a need for it.

Deferring these avoids silently expanding the reviewed V1 protocol or changing
the signed Task Contract after review.
