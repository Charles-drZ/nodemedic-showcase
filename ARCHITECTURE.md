# NodeMedic Architecture Overview

NodeMedic starts local-first. The first useful product does not require a NodeMedic cloud account.

```text
┌──────────────────────── Pi Node host ────────────────────────┐
│                                                              │
│  Pi Node / Docker                                            │
│       │                                                      │
│       ▼                                                      │
│  NodeMedic collectors                                       │
│       │                                                      │
│       ▼                                                      │
│  Normalized observations                                     │
│       │                                                      │
│       ▼                                                      │
│  Deterministic diagnosis rules                               │
│       │                                                      │
│       ▼                                                      │
│  Findings: severity + evidence + confidence + next action    │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

A later optional cloud layer may provide external reachability probes, incident monitoring, notifications, remote status, and Pi-native paid functionality.

The local Doctor remains useful without that cloud layer.

## Why local-first?

- Node diagnostics naturally require local host, Docker, and network observations.
- The first release can work without dedicated production infrastructure.
- Sensitive operational data stays on the operator's machine by default.
- The free local tool can prove real value before a paid monitoring service is built.

## Security boundaries

The MVP is read-only and is explicitly not a wallet tool. NodeMedic must never require a Pi wallet seed phrase or private key.
