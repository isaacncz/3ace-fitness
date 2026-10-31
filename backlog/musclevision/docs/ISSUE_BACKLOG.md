# 3ACE MuscleVision — GitHub Coding Agent Issue Backlog

Copy any task below into a GitHub Issue assigned to your coding agent. Every task inherits `docs/PRD.md` and root `AGENTS.md` requirements.

## MV-001 [P0] Bootstrap offline-capable React PWA and CI
**Depends on:** none. **Deliverables:** Vite + React + TS strict, `pnpm` scripts (`dev`, `build`, `lint`, `typecheck`, `test`), `.github/workflows/ci.yml`, basic responsive screen shell, manifest, placeholder silhouette, client-side routing. **Acceptance:** static HTTPS preview opens in iPad Safari and offers Add to Home Screen; `pnpm lint && pnpm typecheck && pnpm test && pnpm build` succeeds; no runtime CDN dependencies. Do not claim anatomical or offline-pack readiness.

## MV-002 [P0] Audit anatomy model provenance and licensing
**Depends on:** none. **Deliverables:** `docs/ASSET_PROVENANCE.md`, source and license table per relevant mesh, screenshot of source/Blender scene, identification of actual meshes required for 12 groups, documented derivative obligations. **Acceptance:** no assumptions about contents of downloaded archives; no third-party noncommercial/conflicting assets redistributed; annotate blockers. Release asset cannot proceed until sign-off.

## MV-003 [P0] Benchmark representative 3D scene on M1 iPad
**Depends on:** MV-001 and candidate model from MV-002. **Deliverables:** draft 3D scene, orbit + pinch, on-device FPS and memory measurements, recommended polygon/texture/compression budgets. **Acceptance:** measurements explicitly state physical device and iPadOS; compare compressed/uncompressed or two reasonable configurations; preserve conservative fallback.

## MV-004 [P1] Define typed catalogs and automated cross-reference validators
**Depends on:** MV-001. **Deliverables:** types, `muscles.json` (12 groups), `exercises.json` (10 exercises), schema test, aliases and role labels, review notes. **Acceptance:** CI fails on duplicate IDs, missing referenced groups, missing primary role, invalid literals. No implied EMG percentages.

## MV-005 [P1] Blender export, mesh registry and asset manifest
**Depends on:** MV-002. **Deliverables:** authored/exported named GLB, conversion instructions/scripts, mesh-ID registry, mesh report, model bounds, SHA-256 pack manifest, integrity validators. **Acceptance:** each group resolves to source-confirmed GLB IDs; no mirrored left-right confusion; pack provenance audited; final assets marked blocked if license pending.

## MV-006 [P1] Build 3D anatomy viewer controls
**Depends on:** MV-003, MV-005. **Deliverables:** responsive anatomy canvas, orbit/pinch, 4 camera presets/reset, local GLB loader, error/fallback UI. **Acceptance:** no model reload on panel changes; real iPad controls verified or marked pending; invalid asset displays recoverable error.

## MV-007 [P1] Implement deterministic muscle picking and highlight
**Depends on:** MV-004, MV-006. **Deliverables:** mesh-to-group registry, tap threshold, raycast selection, show all/isolate, distinct material states. **Acceptance:** 12 selectable groups, no accidental tap from dragging, no leaked materials across toggles.

## MV-008 [P1] Add muscle details and offline search
**Depends on:** MV-004, MV-007. **Deliverables:** contextual detail rail/bottom sheet, local search by name and alias, muscle info/actions, related exercise links, no-result state. **Acceptance:** panel/text/model IDs agree; search makes no network request; compact virtual keyboard use validated.

## MV-009 [P1] Exercise catalog and role highlight mode
**Depends on:** MV-004, MV-007. **Deliverables:** exercise search/categories, primary/secondary/stabilizer legend, multi-group highlights, contextual muscle details. **Acceptance:** all 10 exercises work and accurately reference model groups; selecting a muscle in exercise context preserves selected exercise; no activation score or percentage shown.

## MV-010 [P1] Implement app-shell Service Worker
**Depends on:** MV-001. **Deliverables:** Vite PWA injectManifest, offline navigation fallback, versioned shell caches, self-hosted fonts and decoder assets, registration status. **Acceptance:** shell opens after relaunch in Airplane Mode without downloaded anatomy pack; does not falsely say model is offline ready.

## MV-011 [P1] Implement versioned atomic anatomy pack installer
**Depends on:** MV-005, MV-010. **Deliverables:** user-triggered downloader, progress bytes, cache staging, response length/SHA-256 and model ID checks, IndexedDB active pointer, network/quota/checksum errors. **Acceptance:** interrupted install not marked ready, no partially active pack, tests OF-01 and OF-04/05/06 pass; no third-party runtime fetch.

## MV-012 [P1] Add offline diagnostics, repair and safe upgrades
**Depends on:** MV-011. **Deliverables:** Verify/Repair/update UI, storage estimates and persistence request, rollback/old-pack retention, readiness and corruption states, install instructions. **Acceptance:** OF-02 through OF-12; physical iPad standalone/Airplane Mode relaunch and reboot test documented.

## MV-013 [P2] Polish touch accessibility and M1 performance
**Depends on:** MV-006 through MV-012. **Deliverables:** landscape/portrait responsive layout, ≥44 CSS px controls, reduced motion, app safe-area handling, pixel ratio/shadow tuning, memory/context-loss recovery. **Acceptance:** 20-minute device demo without crash; frame rate benchmark recorded; desktop automated checks and iPad manual checks documented separately.

## MV-014 [P0 release gate] Verify educational content and attributions
**Depends on:** MV-004, MV-005, MV-009. **Deliverables:** reviewed anatomical functions, exercises and technique caveats, complete About/attribution text, source links, derivative-model sharing obligations. **Acceptance:** expert content review and provenance reviewer sign-off; no unsupported activation claims.

## MV-015 [P0 release gate] Ship v1.0 with offline certification
**Depends on:** all above. **Deliverables:** deployment runbook, rollback procedure, manifest/version compatibility notes, install guide, device checklist and release tag. **Acceptance:** PRD §12 completed, all OFFLINE/3D/QA/license acceptance tests logged; no pending critical issue concealed in release notes.

## Coding-agent starter prompt

> Read `AGENTS.md` and `docs/PRD.md` completely. Implement **MV-001 only** in a small pull request. Do not begin other issue scopes. Use a React + TypeScript + Vite project with an iPad-first PWA shell, an explicitly marked 3D placeholder, and GitHub CI. Add tests, run `pnpm lint`, `pnpm typecheck`, `pnpm test`, `pnpm build`, and report each result. Do not claim asset licensing or true offline anatomy readiness yet. Include a summary, changed files, how to preview, and unverified physical-iPad tests in the PR description.
