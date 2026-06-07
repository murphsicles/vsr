# vsr — Viewstamped Replication consensus for Zeta

**Namespace:** `@consensus/vsr`

Viewstamped Replication (VSR) consensus protocol implementation.
Replica state machine, view change, prepare/commit log replication,
and quorum-based fault tolerance.

## Protocol

```
Client → Primary: REQUEST
Primary → Followers: PREPARE
Followers → Primary: PREPARE_OK
Primary → All: COMMIT (after quorum)
Primary → Client: REPLY

On failure:
Follower → All: START_VIEW_CHANGE
Candidates → All: DO_VIEW_CHANGE
New Primary → All: START_VIEW
```

## Key structs

| Struct | Purpose |
|--------|---------|
| `Replica` | Core state machine (view, role, op, commit) |
| `LogEntry` | Prepared operation with checksum |
| `Prepare` | Primary-to-follower replication message |
| `ViewChangeState` | View change tracking |

## API

- `Replica_new(id, cluster_size)` — create replica
- `on_request(request, client)` — process client request on primary
- `on_prepare(prepare)` — follower receives prepare
- `on_prepare_ok(sender, slot, view)` — primary counts votes
- `start_view_change()` — initiate view change
- `do_view_change(new_view, log, sender)` — cast vote
- `start_view(new_view, op, commit)` — accept new view

## License

MIT — The Zeta Foundation
