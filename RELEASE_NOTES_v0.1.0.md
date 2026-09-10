# NodeMedic v0.1.0 — Draft Release Notes

> Status: **draft / not released**

`v0.1.0` is intended to be the first public-quality NodeMedic Doctor release.

## What is already implemented

- local Go CLI
- `nodemedic scan`
- macOS/Linux host resource observations
- Docker CLI/daemon observations
- Pi Node container discovery
- official Pi Node `node-status` evidence integration
- local Pi-port and Docker-mapping observations
- deterministic evidence-backed findings
- terminal reports
- JSON reports
- sanitized support-bundle preview/export
- local `make install` / `make uninstall` workflow
- deterministic rule, collector, report, scan, support-bundle, and install-workflow tests

## Diagnostic rules currently implemented

- `D001` Docker unavailable
- `D002` Pi Node container stopped
- `D004` mapped local Pi port not listening
- `D005` missing/inconsistent Docker port mapping
- `D008` disk pressure
- `D009` memory pressure
- `D010` synced/running node with zero authenticated peers under conservative evidence conditions

## Deliberately not claimed yet

The following need additional evidence before they can be promoted as implemented diagnoses:

- restart-loop diagnosis — requires local history/trend evidence
- external reachability failure — requires a real external probe
- CGNAT diagnosis — requires external/public/WAN address correlation

## Runtime proof available

The first real public runtime demo was captured on September 10, 2026 from `0.1.0-dev` on macOS arm64.

It demonstrated:

- normal command-line installation
- a real `nodemedic scan`
- evidence-backed `D001` and `D008` findings
- explicit unavailable evidence instead of guessed Pi Node status
- sanitized support-bundle export

See [DEMO.md](DEMO.md).

## Remaining release validation

Before declaring `v0.1.0` ready, the project should still validate the Doctor on a real Pi Node host so the following path is proven together:

```text
Docker → Pi container discovery → node-status → peer/sync evidence → local Pi ports → diagnosis → report
```

Release notes must be updated from this draft when that validation is complete. No release should claim external reachability, CGNAT, history, cloud monitoring, automatic repair, Pi authentication, or Pi payments unless those features are actually implemented and validated.
