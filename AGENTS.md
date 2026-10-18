# 3ACE MuscleVision — Instructions for Coding Agents

This file governs GitHub coding-agent work on 3ACE MuscleVision. Read `docs/PRD.md` (or `PRD_3ACE_MuscleVision_v1.md` in handoff package) before editing.

## Product constraints

- M1 iPad, Safari Home Screen PWA, offline-first. React + TS + Vite + React Three Fiber + Three.js. No backend, login, camera or external runtime APIs in MVP.
- 12 muscle groups, 10 exercises. Never imply actual measured muscle activation.
- Offline-ready status must be based on verified complete asset pack; app shell alone is not enough.
- Model provenance/licensing (especially possible CC BY-SA derivatives) must be audited before any asset ships.

## Implementation rules

1. One numbered `MV-###` issue per PR. Make the smallest testable change.
2. Preserve semantic IDs; UI labels and GLB mesh source names may change independently.
3. Keep external dependencies self-hosted and available offline, including decoders and fonts.
4. Use a versioned asset manifest with SHA-256 and byte length. Stage large files before marking active. A failed update must not delete a working pack.
5. Never claim real-iPad/manual tests were run unless they were actually performed. Record pending QA honestly.
6. For GLB selection, explicitly validate mesh registry IDs. Do not silently infer from arbitrary mesh names.
7. No unreviewed or unlicensed anatomy models should be committed as release assets. Developer placeholders must be visibly flagged.
8. Accessibility: 44px touch targets, meaningful labels, non-color role distinction, reduced motion.

## PR checklist

- [ ] Issue scope and dependencies stated
- [ ] Implementation and unit/E2E tests included
- [ ] `pnpm lint`, `pnpm typecheck`, `pnpm test`, `pnpm build` pass
- [ ] Existing offline flows unaffected
- [ ] No third-party runtime network request introduced
- [ ] Screenshots/recording included for visuals
- [ ] iPad validation recorded as done or pending (with precise reason)
- [ ] License/attribution impacts documented

## Release blockers

No asset pack license audit, missing published muscle mesh, failed Airplane Mode relaunch/reboot, corrupt ready-state semantics, or unverified model accuracy => release must be blocked.
