# NodeMedic Architecture Overview

NodeMedic is local-first: diagnosis remains useful without a NodeMedic account, Cloud connection, or paid service.

The architecture now has two intentionally separate layers:

1. the local Doctor/reliability loop;
2. an outbound-only Agent and Cloud foundation for later remote visibility.

## Local product architecture

```text
┌──────────────────────── Node operator host ────────────────────────┐
│                                                                   │
│  Host / Docker / Pi Node / local network evidence                 │
│                       │                                           │
│                       ▼                                           │
│                  Collectors                                       │
│                       │                                           │
│                       ▼                                           │
│              Normalized observations                              │
│                       │                                           │
│                       ▼                                           │
│           Deterministic diagnosis rules                           │
│                       │                                           │
│                       ▼                                           │
│       Findings + severity + evidence + confidence                 │
│                       │                                           │
│             ┌─────────┴──────────┐                                │
│             ▼                    ▼                                │
│      Terminal / JSON        SQLite history                        │
│                                  │                                │
│                         ┌────────┴────────┐                       │
│                         ▼                 ▼                       │
│                    Local API        Scheduler / transitions       │
│                         │                                         │
│                         ▼                                         │
│                 Embedded dashboard                                │
│                                                                   │
└───────────────────────────────────────────────────────────────────┘
```

The dashboard does not own a second diagnosis model. It renders persisted and live contracts from the same Doctor engine.

The local HTTP service is loopback-only by design.

## Managed Linux operation

On Linux the local product can run as a rootless `systemd --user` service.

The service model preserves several boundaries:

- no automatic root escalation;
- no automatic Docker-group changes;
- no public/LAN listener by default;
- local history remains user-owned data;
- normal upgrade/uninstall preserves history;
- explicit purge is separate from uninstall;
- systemd journal output is used rather than introducing a new raw-log collection system.

## Cloud / Agent trust boundary

The future remote-visibility layer does **not** turn the local HTTP service into an inbound administration API.

```text
Pi Platform identity
        │
        ▼
NodeMedic browser session
        │
        ▼
Account-bound node registration
        │
        ▼
Short-lived one-time enrollment token
        │
        ▼
Local Agent credential
        │
        ▼
Authenticated outbound Agent communication
        │
        ▼
NodeMedic Cloud
```

Browser, Pi, and Agent authentication are separate scopes. Agent credentials are node/instance-oriented and are not the same thing as a Pi access token or browser session.

The implemented foundation currently reaches authenticated Agent heartbeat. Complete health-state synchronization and production Cloud deployment are not claimed yet.

## Why outbound-only?

The Agent naturally needs local host and Docker visibility, but remote monitoring does not require exposing a new inbound administration surface on the node operator's network.

Outbound-only communication keeps several properties intact:

- the local Doctor can continue during Cloud outages;
- local HTTP remains loopback-only;
- router/port-forwarding changes are not required for the Agent;
- remote shell and automatic remediation stay outside the trust boundary;
- Cloud compromise does not automatically become arbitrary host-command authority.

## Evidence and ordering

Remote health data requires stronger guarantees than “the latest request timestamp”. The Cloud architecture therefore treats authenticated Agent instance identity, event identity, ordering/sequence, freshness, and stale/offline semantics as explicit protocol concerns.

Older or replayed state must not be able to roll current health backward silently.

## External probes

External reachability cannot be inferred reliably from local port state alone. A later Cloud probe layer is therefore separated from local diagnosis.

Probe targets must be associated with a verified node/operator context rather than allowing NodeMedic to become an arbitrary network-probing service. Special-address handling, DNS behavior, and actual connection targets are security concerns, not just input-validation details.

## Security boundaries

NodeMedic is explicitly not a wallet tool.

Core principles include:

- never require a Pi wallet seed phrase or private key;
- keep the local diagnostic baseline read-only;
- keep raw logs/support bundles out of default Cloud upload;
- keep browser and Agent credentials separate;
- do not expose a generic remote shell or automatic repair path;
- fail visibly when evidence is unavailable instead of manufacturing a confident diagnosis.

## Product principle

Cloud is an additional visibility layer. It must not become a dependency for the basic value proposition: **a node operator should still be able to run NodeMedic locally and understand why the node is unhealthy.**
