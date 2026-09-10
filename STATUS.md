# Development Status

Current phase: **M1 — Local Node Doctor**

## Current focus

The working NodeMedic CLI now collects local host/Docker/Pi Node evidence, evaluates deterministic diagnostic rules, renders terminal/JSON reports, and exports sanitized support bundles.

The primary command is:

```bash
nodemedic scan
```

## What exists today

- Go single-binary CLI
- macOS/Linux host resource collection
- Docker availability/daemon evidence
- Pi Node container discovery
- official Pi Node `node-status` evidence integration
- local Pi-port and Docker-mapping observations
- evidence-backed diagnostic rules
- terminal and JSON reports
- sanitized support-bundle preview/export
- local install/uninstall workflow
- deterministic unit/runtime tests

## First real public proof

On September 10, 2026, the real `0.1.0-dev` binary was installed and run on macOS arm64.

The run produced two evidence-backed findings:

- `D001` — Docker unavailable (`ERROR`, high confidence)
- `D008` — disk space pressure (`WARNING`, high confidence)

Because Docker was unavailable, NodeMedic correctly left Pi Node container, `node-status`, peer, and Pi-port evidence unavailable rather than guessing a healthy or failed Pi Node state.

See [DEMO.md](DEMO.md) and [samples/support-bundle-2026-09-10.json](samples/support-bundle-2026-09-10.json).

## Remaining M1 proof points

- validate a real Pi Node host end-to-end
- capture container + `node-status` + peer/sync + local Pi-port evidence together
- add external reachability evidence before claiming external-connectivity or CGNAT diagnosis
- add history before claiming restart-loop diagnosis
- decide the final `v0.1.0` release boundary

## Release status

`v0.1.0` is **not released yet**. A draft release note is maintained in [RELEASE_NOTES_v0.1.0.md](RELEASE_NOTES_v0.1.0.md), with remaining validation called out explicitly.
