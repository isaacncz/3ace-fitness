# 3ACE MuscleVision

*See the muscle. Understand the movement.*

An offline-first 3D muscle anatomy teaching tool for instructors and gym beginners. Rotate a human muscular model, tap a muscle to learn its function, and pick an exercise to see which muscle groups are commonly involved.

**Status:** Planning only. No application code yet. The PRD and issue backlog are the design baseline, and anatomical asset sourcing and licensing are open implementation gates.

## Target

- Primary: Apple M1 iPad, Safari, installed as a Home Screen PWA, fully usable in Airplane Mode after the anatomy pack is installed.
- Secondary: iPhone Safari and desktop browsers (responsive, not acceptance devices).
- Stack: React, TypeScript, Vite, React Three Fiber / Three.js, GLB models, Service Worker + Cache Storage + IndexedDB. No backend, login, camera, analytics or runtime third-party requests.
- Scope: 12 muscle groups, 10 exercises. Exercise colouring shows general anatomical involvement, not measured muscle activation.

## Documentation

| File | Purpose |
| --- | --- |
| [docs/PRD.md](docs/PRD.md) | Product requirements: screens, architecture, data schemas, offline pack design, asset pipeline, test plan, release definition of done |
| [docs/ISSUE_BACKLOG.md](docs/ISSUE_BACKLOG.md) | Phased `MV-001` to `MV-015` issue list with dependencies and acceptance criteria, plus the coding-agent starter prompt |
| [AGENTS.md](AGENTS.md) | Rules for coding agents: one `MV-###` issue per PR, PR checklist, release blockers |

Planned but not yet written: `docs/ASSET_PROVENANCE.md` (MV-002) and `docs/IPAD_TEST_MATRIX.md`.

## Working on this repo

1. Read `AGENTS.md` and `docs/PRD.md` first.
2. Pick one issue from `docs/ISSUE_BACKLOG.md` and keep the PR to that issue's scope. Start with `MV-001` (app bootstrap and CI) and `MV-002` (model licensing audit), which are independent.
3. Every PR must pass `pnpm lint`, `pnpm typecheck`, `pnpm test` and `pnpm build`. Record real-iPad checks as done or pending, never assumed.

## Release blockers

No shipped anatomy model without a completed license and provenance audit. No release without a passing Airplane Mode relaunch and reboot test on a physical iPad.
