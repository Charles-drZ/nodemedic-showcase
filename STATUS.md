# Development Status

NodeMedic is currently in **M1 release validation**, with the **M2 local reliability loop implemented in parallel** and the first **Cloud/Agent foundation** under active development.

The project deliberately separates implementation progress from release claims: a capability can exist in the private engineering repository without being presented as production-proven until the required runtime validation has happened.

## Local Doctor

The primary command remains:

```bash
nodemedic scan
```

The working Doctor can collect host, Docker, Pi Node, networking, port, storage, and resource observations; evaluate deterministic diagnostic rules; render terminal/JSON reports; and export sanitized support bundles.

Implemented local foundations include:

- Go single-binary application;
- macOS/Linux host resource collection;
- Docker availability and daemon evidence;
- Pi Node container discovery;
- official Pi Node `node-status` evidence integration;
- local Pi-port and Docker-mapping observations;
- evidence-backed diagnostic rules;
- terminal and JSON reports;
- sanitized support-bundle preview/export;
- local install/uninstall workflow;
- automated diagnostic/runtime tests;
- deterministic multi-platform release packaging foundation.

## M2 local reliability loop

The private engineering repository also contains the working local product loop around the Doctor:

- versioned SQLite scan history;
- bounded retention;
- restart-safe latest/history queries;
- local health-transition persistence;
- optional scheduled scans;
- serialized manual and scheduled Doctor execution;
- loopback-only local HTTP API;
- embedded responsive dashboard;
- manual diagnosis from the dashboard;
- rootless `systemd --user` service lifecycle;
- upgrade/uninstall behavior that preserves local history unless purge is explicit.

These capabilities do not change the remaining M1 real-host proof gate.

## Cloud / Agent foundation

Implemented foundation work currently includes:

- Pi Platform identity verification;
- Pi Browser authentication bootstrap;
- short-lived NodeMedic browser sessions;
- account-bound node registration;
- short-lived single-use Agent enrollment tokens;
- separate Agent credentials;
- interactive local Agent enrollment with protected credential storage;
- authenticated one-shot Agent heartbeat.

**Not yet claimed:** complete Cloud state synchronization, background Agent sync, production Cloud deployment, remote repair, payments, or production multi-node monitoring.

Heartbeat proves authenticated Agent liveness only; it is not treated as Pi Node health.

## First public runtime proof

On September 10, 2026, the real `0.1.0-dev` binary was installed and run on macOS arm64.

The run produced evidence-backed findings including Docker unavailability and disk-space pressure. Because Docker was unavailable, NodeMedic correctly left Pi Node container, `node-status`, peer, and Pi-port evidence unavailable rather than guessing a healthy or failed Pi Node state.

See [DEMO.md](DEMO.md) and [samples/support-bundle-2026-09-10.json](samples/support-bundle-2026-09-10.json).

## Remaining important proof points

Before treating the initial Doctor release as fully proven:

- validate the Doctor on a real Pi Node host end-to-end;
- capture container + `node-status` + peer/sync + local Pi-port evidence together;
- keep unsupported external-connectivity/CGNAT claims disabled until real external-probe evidence exists;
- complete the intended release validation matrix;
- decide and validate the final `v0.1.0` release boundary.

## Release status

`v0.1.0` is **not released yet**.

Cross-platform packaging support and local product capabilities are development/release evidence, not substitutes for the remaining real Pi Node host validation gate.
