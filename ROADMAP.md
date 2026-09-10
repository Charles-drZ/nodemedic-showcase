# NodeMedic Public Roadmap

## M0 — Product & Technical Feasibility

**Completed foundation.**

Defined the diagnostic model, security boundaries, initial Pi Node failure scenarios, local-first architecture, and private-engineering/public-showcase split.

## M1 — Local Node Doctor

**Active.**

Working command:

```bash
nodemedic scan
```

Implemented:

- host/Docker/Pi Node evidence collection
- official `node-status` integration
- local Pi-port and Docker-mapping observations
- deterministic diagnostic rules
- terminal and JSON reports
- sanitized support-bundle export
- local install/uninstall workflow
- real runtime demo on macOS arm64

Public evidence:

- [first real terminal demo](DEMO.md)
- [sanitized support bundle](samples/support-bundle-2026-09-10.json)
- [architecture overview](ARCHITECTURE.md)
- [development status](STATUS.md)
- [draft v0.1.0 release notes](RELEASE_NOTES_v0.1.0.md)

Remaining before treating M1 as fully proven:

- run the Doctor on a real Pi Node host end-to-end
- validate Pi container + `node-status` + peer/sync + local port evidence together
- keep external reachability/CGNAT claims disabled until an external probe exists
- keep restart-loop claims disabled until history exists

## M2 — Local History & Dashboard

Add local history, incident timeline, and a browser-based local dashboard.

## M3 — NodeMedic Watch

Validate optional remote monitoring, external reachability checks, incident detection, and notifications.

## M4 — Pi-native Integration

If real usage validates the product, add Pi authentication, Pi Browser experience, Pi payments, and suitable SoloHost integration.

## Principle

Cloud and monetization come after the local Doctor proves useful to real Pi Node operators.
