# NodeMedic Public Roadmap

The roadmap tracks product proof, not just implementation order. Later-layer work can be developed in parallel without weakening earlier release gates.

## M0 — Product & Technical Feasibility

**Completed.**

Defined the local-first diagnostic model, initial Pi Node failure scenarios, security boundaries, public/private repository split, and evidence-backed diagnosis principles.

## M1 — Local Node Doctor

**Implemented core; release validation remains active.**

Working command:

```bash
nodemedic scan
```

Implemented foundations include host/Docker/Pi Node evidence collection, official `node-status` integration, local Pi-port observations, deterministic diagnostic rules, terminal/JSON reporting, sanitized support bundles, installation tooling, automated tests, and multi-platform release packaging.

Remaining proof is intentionally narrower than the amount of code already implemented:

- real Pi Node host validation end-to-end;
- combined container + `node-status` + peer/sync + local-port evidence;
- external reachability evidence before external-connectivity or CGNAT diagnosis is claimed;
- final release-validation matrix and `v0.1.0` boundary.

Public evidence:

- [first real terminal demo](DEMO.md)
- [sanitized support bundle](samples/support-bundle-2026-09-10.json)
- [architecture overview](ARCHITECTURE.md)
- [development status](STATUS.md)
- [draft v0.1.0 release notes](RELEASE_NOTES_v0.1.0.md)

## M2 — Local Reliability Loop

**Working implementation exists.**

The Doctor has been extended into a local reliability product with:

- SQLite-backed scan history;
- bounded retention;
- health-transition records;
- optional recurring scans;
- loopback-only local API;
- embedded responsive dashboard;
- rootless Linux `systemd --user` service operation;
- history-preserving upgrade/uninstall behavior.

This work is developed in parallel and does not replace the remaining M1 real-host validation gate.

## M3 — Cloud / Watch Foundation

**Foundation in progress. No production Cloud deployment claimed.**

The architecture keeps the local Agent outbound-only and preserves the local Doctor when Cloud is unavailable.

Implemented foundation currently includes:

- Pi identity verification;
- Pi Browser authentication bootstrap;
- short-lived NodeMedic browser sessions;
- account-bound node registration;
- single-use Agent enrollment;
- protected local Agent credentials;
- authenticated Agent heartbeat.

Next proof points include completing the state-synchronization contract, durable Cloud persistence, external-probe behavior, freshness/offline semantics, and production deployment validation.

Remote repair and inbound Agent administration remain outside the intended boundary.

## M4 — Product Expansion & Pi-native Value

Later hypotheses include remote monitoring, notifications, multi-node views, Pi-native paid features, and suitable ecosystem integrations.

These should be driven by real operator usage rather than implemented solely because the platform allows them.

## Principle

The free local Doctor must remain useful on its own. Cloud, monitoring, and monetization are additional product layers, not dependencies for basic diagnosis.
