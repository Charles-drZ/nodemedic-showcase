# NodeMedic

**Pi Node Doctor & Watchdog**

NodeMedic is a local-first diagnostics and reliability toolkit for Pi Network Node operators.

> Know why your Pi Node is not healthy.

## The problem

A node can appear degraded for many different reasons: Docker state, service failures, local port exposure, router/ISP connectivity, CGNAT, storage pressure, memory pressure, sync state, or peer connectivity. Operators often have to correlate several tools and community reports before they can identify the actual cause.

NodeMedic turns structured local observations into deterministic, evidence-backed findings with severity, confidence, likely cause, and a recommended next step.

## Real runtime evidence

The first public runtime proof was captured on **September 10, 2026** from the real `0.1.0-dev` binary on macOS arm64.

```text
$ nodemedic scan
NODEMEDIC
Overall: UNHEALTHY

Core status
Docker daemon:         UNAVAILABLE
Pi Node container:     NOT OBSERVED
Horizon:               NOT OBSERVED
Authenticated peers:   NOT OBSERVED
Disk pressure:         WARNING
Memory pressure:       HEALTHY
Port 31401:            NOT OBSERVED
Port 31402:            NOT OBSERVED
Port 31403:            NOT OBSERVED

Findings

D001 [ERROR] — Docker is unavailable
NodeMedic cannot inspect the Pi Node container because the Docker CLI is not available on the host.
Confidence: HIGH
Next: Install or restore Docker access, then rerun NodeMedic before troubleshooting Pi Node itself.

D008 [WARNING] — Disk space pressure
Insufficient free disk space can destabilize Pi Node or prevent synchronization from progressing.
Confidence: HIGH
Evidence:
  x host.disk.pressure = WARNING (expected HEALTHY)
  x host.disk.available_percent = 9.2 (expected at or above configured threshold)
Next: Free disk space or move node data to a filesystem with more capacity, then rerun the scan.
```

This is not a mock. It is output from the working binary. The host intentionally did **not** have Docker available, so Pi-container, Horizon, peer, and local Pi-port evidence remained `NOT OBSERVED` / `UNAVAILABLE` instead of being guessed.

See [First real demo](DEMO.md) and the [sanitized sample support bundle](samples/support-bundle-2026-09-10.json).

## What works today

- local host resource collection on macOS/Linux
- Docker availability and daemon evidence
- Pi Node container discovery
- official Pi Node `node-status` evidence integration
- local Pi-port / Docker-mapping correlation
- deterministic diagnostic rules
- terminal and JSON reports
- sanitized support-bundle preview/export
- local `make install` / `make uninstall` workflow

The working primary command is:

```bash
nodemedic scan
```

## What is not claimed yet

The first public demo validates the real scan/report pipeline, but it is **not yet proof of a complete healthy Pi Node run**. External reachability and CGNAT diagnosis still require a real external probe, and historical restart-loop diagnosis requires local history.

NodeMedic deliberately leaves unsupported evidence visible instead of inventing a diagnosis.

## Product layers

**Doctor** diagnoses why a node is unhealthy.

**Watchdog** will later detect outages and degradation over time.

**Optimizer** may later help operators understand reliability and operating efficiency.

## Design principles

- Local diagnostics work without an account.
- The MVP is read-only.
- Findings show evidence and confidence, not opaque guesses.
- NodeMedic never requires a Pi wallet seed phrase or private key.
- Paid/cloud features come only after the free local Doctor proves useful.

## Architecture

The local CLI is implemented in Go and separates collection from diagnosis:

```text
Collectors → normalized observations → diagnostic rules → findings → reports
```

See [Architecture](ARCHITECTURE.md).

## Roadmap

NodeMedic is currently in **M1 — Local Node Doctor** development.

The next proof point is a real Pi Node host run covering container discovery, `node-status`, peer/sync evidence, and Pi-port observations end-to-end.

See [Roadmap](ROADMAP.md) and [Development status](STATUS.md).

## Public showcase policy

This repository documents the product, architecture, runtime evidence, release progress, and selected engineering decisions. The private engineering repository remains the implementation source of truth during early validation.

The showcase does not present mocks as implemented behavior. Demo artifacts are labeled with their actual environment and limitations.

## Disclaimer

NodeMedic is an independent project and is not affiliated with or endorsed by Pi Network or the Pi Core Team.
