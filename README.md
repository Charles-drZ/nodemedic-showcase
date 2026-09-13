[← Developer profile](https://github.com/Charles-drZ)

# NodeMedic

**Local-first diagnostics and reliability tooling for Pi Network Nodes.**

> Diagnose what is actually wrong, show the evidence, and avoid pretending that unavailable data is known.

NodeMedic started as a local Node Doctor and has grown into a broader reliability product: deterministic diagnostics, persisted health history, a local dashboard and API, recurring health checks, managed Linux operation, release packaging, and the foundations of an outbound-only Cloud/Agent model.

The implementation repository remains private during product validation. This public repository tracks verified behavior, architecture, runtime evidence, and selected engineering decisions without publishing wallet-sensitive, infrastructure-sensitive, or proprietary implementation details.

## Current engineering scope

### Local Doctor

The working Go binary collects host, Docker, Pi Node, networking, port, storage, and resource observations and evaluates deterministic diagnostic rules.

Findings include:

- severity and confidence;
- concrete supporting evidence;
- likely cause;
- a recommended next step;
- explicit `NOT OBSERVED` / unavailable states when evidence cannot be collected.

Primary local workflow:

```bash
nodemedic scan
```

Reports are available for terminal use and structured JSON processing, with a sanitized support-bundle path for sharing diagnostic evidence without blindly exporting the host environment.

### Local reliability loop

NodeMedic now has a real local product surface around the Doctor engine:

- versioned SQLite scan history;
- bounded retention;
- health-transition persistence;
- optional scheduled scans;
- a loopback-only local HTTP API;
- an embedded responsive dashboard served from the Go binary;
- manual diagnosis from the dashboard;
- restart-safe local history;
- serialized manual and scheduled Doctor execution.

The dashboard renders the real Doctor contracts. It does not fabricate uptime, external reachability, incidents, cloud state, or other data that NodeMedic has not actually observed.

### Managed Linux runtime

On Linux, NodeMedic can run as a rootless `systemd --user` service.

The managed lifecycle includes install, upgrade, start, stop, restart, status, journal-backed logs, uninstall, and explicit data purge behavior. Normal upgrade and uninstall preserve local SQLite history, and the service remains loopback-only by default.

The runtime deliberately avoids automatic privilege escalation, Docker-group mutation, public listeners, remote shell capability, and automatic repair.

### Release engineering

The private engineering repository includes deterministic release packaging for the initial multi-platform matrix, including static binaries, linker-injected version information, SHA-256 manifests, packaging validation, and platform-safe shutdown behavior.

Cross-compilation is treated as build evidence, not as a substitute for real-host runtime certification.

## Cloud and Agent foundation

The next product layer is designed around an **outbound-only Agent**. The local Doctor remains independently useful even if Cloud services are unavailable.

Implemented foundation work currently covers:

- Pi Platform identity verification at the server boundary;
- Pi Browser authentication bootstrap;
- short-lived NodeMedic-owned browser sessions;
- account-bound node registration;
- short-lived, single-use Agent enrollment tokens;
- separate Agent credentials rather than reusing browser or Pi credentials;
- interactive local Agent enrollment with protected credential storage;
- authenticated one-shot Agent heartbeat;
- revocation/supersession boundaries for Agent instances.

This is deliberately staged work. **Cloud state synchronization is not claimed as complete, and no production Cloud deployment is claimed here.** Heartbeat represents authenticated Agent liveness, not Pi Node health.

## Architecture

The local diagnosis path keeps evidence collection separate from interpretation:

```text
Collectors
    ↓
Normalized observations
    ↓
Deterministic diagnostic rules
    ↓
Findings + report
    ↓
SQLite history / local API / dashboard / scheduled checks
```

The Cloud direction adds a separate trust boundary rather than exposing the local NodeMedic service inbound:

```text
Pi identity → NodeMedic browser session → node registration
                                      ↓
                              one-time enrollment
                                      ↓
Local Agent credential → authenticated outbound communication
```

The local Doctor does not depend on Cloud account, billing, or network availability to perform local diagnosis.

## Security boundaries

NodeMedic is intentionally conservative around a system that may share a host with a blockchain node.

Key principles include:

- no Pi wallet seed phrase or private-key access;
- read-only diagnosis as the local baseline;
- loopback-only local HTTP service;
- no automatic repair or remote shell;
- browser, Pi, and Agent credentials remain separate scopes;
- enrollment secrets are not designed for normal command-line argument or environment-variable exposure;
- raw logs and complete environment dumps are not default Cloud payloads;
- stale or missing evidence must remain visible rather than being converted into confident health claims.

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
```

The host intentionally did not have Docker available. NodeMedic therefore left Pi-container, Horizon, peer, and Pi-port evidence unobserved instead of guessing a result.

See [First real demo](DEMO.md) and the [sanitized sample support bundle](samples/support-bundle-2026-09-10.json).

## What this project demonstrates

NodeMedic combines several engineering concerns inside one product:

- Go application and systems development;
- deterministic diagnostics and explicit uncertainty;
- local persistence and migration-safe history;
- HTTP API and embedded product UI;
- scheduler and service lifecycle design;
- Linux rootless operation;
- release artifact engineering;
- authentication and credential-scope boundaries;
- staged local-to-cloud architecture;
- security documentation and threat modeling;
- validation that distinguishes build support from real runtime proof.

## Product direction

**Doctor** diagnoses why a node is unhealthy.

**Watchdog** tracks degradation and health changes over time.

**Cloud** can later add remote visibility and carefully bounded monitoring without turning the Agent into an inbound administration surface.

**Optimizer** remains a later hypothesis for helping operators understand reliability and operating efficiency.

Paid or Pi-native features come after the free local product proves useful.

## Public boundary

This repository does not publish the private source tree, deployable Cloud configuration, credentials, internal test fixtures, private infrastructure, exact production endpoints, wallet material, or raw host data.

Public claims are intentionally narrower than private implementation when validation is incomplete. Mocks and architecture plans are not presented as production behavior.

## Project status

The Local Doctor and the local reliability loop are working engineering systems. Release validation and real Pi Node host proof remain important gates. Cloud/Agent work is being built incrementally behind explicit trust boundaries; production Cloud sync is not yet claimed.

## Disclaimer

NodeMedic is an independent project and is not affiliated with or endorsed by Pi Network or the Pi Core Team.
