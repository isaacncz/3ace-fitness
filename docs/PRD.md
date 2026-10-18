# 3ACE MuscleVision — Implementation-Ready Product Requirements Document

**Version:** 1.0 (MVP)  
**Date:** 2026-10-18  
**Status:** Approved design baseline; anatomical asset acquisition and licensing are implementation gates  
**Primary target:** Apple M1 iPad, latest supported iPadOS/Safari, installed as a Home Screen PWA  
**Secondary target:** iPhone Safari and desktop Chromium/Safari (responsive, not primary acceptance devices)  
**Owner:** 3ACECLUB  
**Repository target:** New `musclevision` project, or isolated `/apps/musclevision` package if added to an existing monorepo

## 1. Product summary

MuscleVision is an offline-first, premium-quality 3D anatomy teaching tool. An instructor or gym beginner can rotate a human muscular model, tap a muscle to learn its function, isolate/highlight its meshes, and choose an exercise to see the muscle groups commonly involved. It must reliably operate on an M1 iPad without an internet connection **after successful offline-pack installation**.

**One-line value proposition:** *See the muscle. Understand the movement.*

### 1.1 Primary user stories

- As an instructor, I can open the installed iPad app with Wi-Fi and cellular disabled and demonstrate muscle anatomy immediately.
- As a beginner, I can touch a muscle on the 3D body and see a short explanation and relevant exercises.
- As an instructor, I can choose an exercise and visualize primary, secondary, and stabilizing muscle-group associations.
- As a user, I can find a named muscle through search, reset to front/back/side views, and isolate my selection.
- As an owner, I can replace anatomical assets or edit educational content without rewriting core selection and rendering logic.

### 1.2 Goals and non-goals

**MVP goals:** Offline-capable 3D viewer; 12 anatomical muscle groups; 10 exercise associations; accurate mesh selection; polished touch UX; verified asset installation; meaningful error/recovery states.

**Not in v1:** Camera access, pose estimation, live AR overlays, exercise form scoring, inferred muscle activation, animation/rigging, personalized workout recommendations, login, backend, analytics SDK, customer data collection, cloud syncing, and medical diagnosis.

### 1.3 Success and quality gates

| Metric | MVP target and measurement |
| --- | --- |
| Offline success | 100% of required v1 interactions pass in Airplane Mode after pack installation, relaunch, and device reboot |
| Correct 3D picks | 100% of 12 published muscle-group IDs have testable selectable meshes; no unrecognized selectable meshes |
| Exercise mappings | 10 exercises with validated primary/secondary/stabilizer relationships |
| Touch ergonomics | Main actionable controls ≥44 × 44 CSS px; pan/rotate/zoom work in Home Screen mode |
| Interaction responsiveness | UI action feedback begins within 150 ms in normal conditions (device-measured; nonblocking animation allowed) |
| Frame rate | Target ≥30 FPS during standard model rotation on M1 iPad; record measured median and 10th percentile |
| Asset size | Preferred core downloadable pack ≤40 MB; report actual asset size and reasons for exceptions |
| Stability | No crashes in a 20-minute continuous demo; no network requests required during installed offline session |
| Content integrity | Every visible label resolves to a stable ID and corresponding mesh or a stated data-only fallback |

Numbers are engineering targets, **not** claims about measured iPad performance.

## 2. Product experience and information architecture

### 2.1 Navigation

- `/` — Viewer (default)
- `/exercises` — Exercise browser, accessible without unmounting/reloading the 3D scene where practical
- `/settings/offline` — Offline installation and diagnostics
- `/about` — Attribution, version, educational disclaimer, help

Prefer a single persistent application shell; viewer panel switches via local UI state so muscle selection does not reload the GLB.

### 2.2 Visual system

- Background: charcoal `#10151D`; panel `#19222D`; high-contrast body copy `#F5F7FA`; muted copy `#AEB8C5`.
- Normal muscles: natural muted muscle red with readable anatomical topology.
- Selected: warm amber `#F5AE52`; secondary: soft blue `#70B7D8`; stabilizer: muted teal `#78B2A1`.
- Do not rely on color alone: pair highlighting with textual muscle names, category badges, and a legend.
- 3D canvas is the visual hero. Limit prose to 1–2 lines initially; optional `Learn more` disclosure.
- iPad landscape layout at ≥900 CSS px: 65–72% viewer, remainder detail rail. Compact widths: full canvas plus collapsible bottom detail sheet.
- Respect safe-area insets, dynamic viewport height, reduced-motion setting, and ≥44 CSS px touch targets.

### 2.3 Screens and acceptance criteria

#### S01 — First-launch / installation

**Entry:** First open with no verified anatomy asset pack.

**Content:** Brand mark, one-sentence value proposition, lightweight built-in silhouette/illustration, pack title/version/byte estimate, prominent `Download for Offline Use`, connectivity/storage info, `How to Add to Home Screen` instructions for iPad.

**Interactions:**
- Tap `Download`: display bytes downloaded, estimated total, current step and cancellation/Retry controls.
- If offline before installation, show explicit `Internet required to download anatomy pack` and never claim ready.
- After asset hashes, manifests, and scene load validate, mark `Offline Ready` and navigate to Viewer.
- If Safari installation is not supported in context, show install instructions rather than a broken programmatic install button.

**Acceptance:** App shell is usable without pack; no false `ready`; retry works after a broken download; completed pack is verified and usable offline.

#### S02 — Interactive anatomy viewer (primary screen)

**Header:** `3ACE MuscleVision`, current mode, pack-ready chip, settings.

**Canvas:** Real 3D human body, default front-three-quarter camera, environmental but self-contained lighting, dark gradient (CSS or generated procedural scene only), simple floor/shadow optional.

**Camera controls:**
- One-finger drag: orbit around target; two-finger pinch: zoom.
- Two-finger drag: pan (optional; if supported, include `Reset View` to recover).
- Presets: `Front`, `Back`, `Left`, `Right`, `Reset`; transitions ~250–450 ms, skipped on reduced motion.
- Zoom limits must prevent inside-body clipping and an unusably small model.
- Tapping a selectable mesh highlights its muscle group. Tapping background clears the current muscle selection, but does **not** silently change exercise mode.
- Raycast only pickable meshes; visible hit priority must be predictable on overlapping muscles.
- Never fetch the 3D model again just to change selection/mode; update materials or per-object visual state.

**Viewer toolbar:** Search, show-all/isolate, front/back, camera reset, optional left/right view dropdown.

**Acceptance:** 12 groups individually selectable; orbit/pinch smooth; state survives layout changes; model stays mounted while panel content changes.

#### S03 — Muscle detail rail / bottom sheet

**Content:** Name, region, laterality if applicable, concise function, common actions, related exercises, `Highlight`, `Isolate`, and `Show all`. One expandable `Learn more` paragraph; no dense encyclopedic copy.

**Interactions:** Selecting from search or tapping the model opens panel. `Isolate` dims/hides nonselected anatomy (default dim to 12–20% opacity; add `Hide Others` toggle only if the rendering supports it). `Show all` restores the normal appearance without losing selected muscle. Related exercise opens exercise mode with matching mapped muscles.

**Acceptance:** Selected ID, panel copy, mesh highlights and URL/deep link state (if enabled) always agree.

#### S04 — Exercise Explorer

**Content:** Searchable catalogue of 10 exercises, category tags (`Push`, `Pull`, `Legs`, `Core`), exercise name, equipment (if relevant), short movement description. Selecting one shows `Primary`, `Secondary`, and `Stabilizer` sections with corresponding colored anatomy groups.

**Interactions:** Selecting exercise clears standalone muscle isolate state, maps exercise roles to highlighted groups, and positions camera to a helpful preset (unless user explicitly locks camera). Tapping a mapped muscle opens its details without losing exercise context; `Back to Exercise` returns to exercise list and preserves selected exercise.

**Acceptance:** All 10 exercise entries reference existing muscle IDs; legend and mesh colors match roles; missing optional roles show no empty section. Never show percentages or claim measured activation.

#### S05 — Search overlay

**Content:** Single input, suggestions for anatomy groups and exercise names, clear button, `No results` empty state.

**Interactions:** Search is local-only and accent/case-insensitive; choosing result closes overlay and focuses appropriate viewer mode. Escape key/back control dismisses; virtual keyboard must not cover the result list.

**Acceptance:** Search works offline; no network or external indexing service; 0-result state is helpful.

#### S06 — Offline & settings

**Content:** App version, current asset-pack version, state (`Not installed`, `Downloading`, `Validating`, `Ready`, `Repair required`, `Update available`), bytes and storage usage estimate, last successful integrity check, `Verify`, `Repair`, `Download update`, `Remove downloaded anatomy pack` (confirmed action), attribution link.

**Interactions:** Request `navigator.storage.persist()` from explicit user interaction when available; display whether request was granted without implying permanence. `Verify` rechecks all expected local resources and hashes. `Repair` is only possible when online. New version installs side-by-side and becomes active **only after full validation**.

**Acceptance:** No ready state unless verified; failed update leaves prior verified version usable; quota/network failures produce useful user-facing messages.

#### S07 — About, licensing and disclaimer

Include app version, asset provenance, exact license/attribution statements, source URLs, relevant modifications to model assets, and link to downloadable share-alike derivatives if licensing requires distribution. Explain that exercise coloring represents general anatomical involvement, **not** measured muscle activation or medical advice.

### 2.4 Explicit UI state transitions

`mode`: `muscle` | `exercise`  
`selectedMuscleGroupId`: `string | null`  
`selectedExerciseId`: `string | null`  
`focusMuscleGroupId`: `string | null` (optional in exercise mode)  
`visibility`: `all` | `isolate`  
`cameraPreset`: `front` | `back` | `left` | `right` | `free`  
`searchQuery`: `string`

Transition rules:
1. Selecting a standalone muscle => `mode=muscle`, set muscle ID, visibility remains unchanged unless the user toggles isolate.
2. Selecting an exercise => `mode=exercise`, set exercise ID, clear standalone isolate; color every mapped muscle by role.
3. Tapping a muscle while exercise selected => show muscle details contextually, preserve exercise selection and highlight roles.
4. `Reset Scene` => default camera, `mode=muscle`, no selection, `visibility=all`.
5. Orientation change => preserve mode and selections; refit camera if necessary, do not reload model.

## 3. Technical architecture

```text
HTTPS static hosting (deployment / updates only)
                |
           First download
                v
M1 iPad Home Screen PWA (React + TypeScript + Vite)
     |                 |                 |
3D renderer          Local domain      Offline Manager
(React Three Fiber)   catalog JSON      (Service Worker,
(Three.js + GLB)      + UI state          Cache API, IndexedDB)
     |                 |                 |
Mesh registry <---- stable IDs ----> model asset manifest
                |
     Verified versioned GLB & decoders
         loaded fully on device
```

### 3.1 Required stack

| Concern | Tool | Notes |
| --- | --- | --- |
| Build/runtime | Vite + React + TypeScript | Strict TS enabled |
| UI state | Zustand or React context | Choose one simple approach; avoid duplicated state |
| 3D | Three.js + `@react-three/fiber` + `@react-three/drei` | OrbitControls, camera transitions, asset loading |
| Model | GLB/glTF 2.0 | Named per-muscle meshes, stable registry |
| Compression | glTF-Transform / Meshopt; optional KTX2 | Choose based on real-device decode/runtime benchmarks |
| Offline shell | `vite-plugin-pwa` + Workbox `injectManifest` | Custom Service Worker for app navigation and asset pack requests |
| Pack storage | Cache Storage API | Explicit versioned caches and fetched bytes |
| Metadata/preferences | IndexedDB (`idb` package) | Atomic active-pack pointer, state, settings |
| QA | Vitest + Testing Library + Playwright | Desktop/WebKit emulation plus real iPad manual gates |
| CI | GitHub Actions | lint, typecheck, test, build, schema/mesh audits |

Avoid fetching from CDNs at runtime (font files, icons, mesh decoder WASM, textures, scripts). Serve all dependencies from the app origin.

### 3.2 Suggested structure

```text
musclevision/
  .github/workflows/ci.yml
  docs/PRD.md
  docs/ASSET_PROVENANCE.md
  docs/IPAD_TEST_MATRIX.md
  public/
    icons/
    fallback-body.svg
    packs/core-v1/
      muscles-core.glb
      pack-manifest.json
  src/
    app/App.tsx
    app/routes.tsx
    components/Header.tsx
    components/AnatomyCanvas.tsx
    components/MuscleDetail.tsx
    components/ExerciseBrowser.tsx
    components/SearchOverlay.tsx
    components/OfflineManagerPanel.tsx
    engine/meshRegistry.ts
    engine/modelLoader.ts
    engine/selection.ts
    engine/cameraPresets.ts
    data/muscles.json
    data/exercises.json
    data/types.ts
    data/validateCatalog.ts
    offline/packInstaller.ts
    offline/packVerifier.ts
    offline/packStore.ts
    offline/useOfflineStatus.ts
    offline/sw.ts
    styles/tokens.css
  scripts/
    prepare-models/README.md
    validate-gltf.mjs
    generate-pack-manifest.mjs
  tests/
    unit/
    e2e/
  package.json
  vite.config.ts
```

The shell should **not** blindly precache a 40 MB GLB. Precache shell, local UI assets, fallback silhouette and decoders necessary for normal operation; fetch large anatomy pack explicitly through the install flow. Ensure any data/decoder needed for viewing is covered in the installed shell or the pack manifest.

## 4. Data schemas and integrity rules

### 4.1 TypeScript domain contracts

```ts
type MuscleGroupId = string;
type ExerciseId = string;
type Role = 'primary' | 'secondary' | 'stabilizer';
type Side = 'left' | 'right' | 'bilateral' | 'midline';
type Region = 'chest' | 'shoulders' | 'arms' | 'back' | 'core' | 'hips' | 'legs';

interface MuscleGroup {
  id: MuscleGroupId;              // stable kebab-case, NEVER presentation name
  label: string;                  // English MVP display label
  anatomicalNames: string[];      // precise structures represented
  region: Region;
  side: Side;
  description: string;            // <= 180 chars for collapsed panel
  actions: string[];              // verified anatomical movement/action labels
  meshIds: string[];              // every ID must exist in packaged GLB
  aliases: string[];              // offline search aliases
  defaultCamera: 'front' | 'back' | 'left' | 'right';
}

interface ExerciseMuscle {
  muscleGroupId: MuscleGroupId;
  role: Role;
  note?: string;                  // when role is technique-dependent
}
interface Exercise {
  id: ExerciseId;
  label: string;
  category: 'push' | 'pull' | 'legs' | 'core';
  equipment: string[];
  description: string;            // <= 180 chars
  techniqueCaveat?: string;
  muscles: ExerciseMuscle[];      // >=1 primary
  suggestedCamera: 'front' | 'back' | 'left' | 'right';
}

interface AssetEntry {
  path: string;                   // same-origin, versioned relative URL
  bytes: number;
  sha256: string;                 // lowercase hex
  mime: string;
}
interface AssetPackManifest {
  schemaVersion: 1;
  id: 'core';
  version: string;                // e.g. '1.0.0'
  minAppVersion: string;
  modelPath: string;
  files: AssetEntry[];            // model + required pack files
  meshIds: string[];              // automated GLB export list
  sourceAttribution: string[];
}

type OfflineState =
  | { status: 'not-installed' }
  | { status: 'downloading'; version: string; receivedBytes: number; totalBytes: number }
  | { status: 'validating'; version: string }
  | { status: 'ready'; version: string; lastVerifiedAt: string }
  | { status: 'update-available'; activeVersion: string; targetVersion: string }
  | { status: 'repair-required'; reason: string }
  | { status: 'error'; code: 'network' | 'quota' | 'checksum' | 'invalid-model' | 'unknown'; message: string };
```

### 4.2 Catalog rules

- IDs remain stable across label translations/model swaps.
- Muscle groups are UI-level categories, not always single discrete muscles: e.g. `quadriceps` may map multiple named quadriceps meshes.
- Map source mesh names into stable `meshIds` using a reviewed registry generated from the actual model, **not** naming heuristics at runtime.
- No exercise references a missing group. No mesh belongs to two selectable groups unless overlaps are intentionally documented and explicit pick priority exists.
- Validate catalog against `pack-manifest.json` and GLB metadata in CI. Fail build or mark asset pack unreleasable on missing/duplicate IDs.
- Show `primary`/`secondary`/`stabilizer` as curated educational categories, not scientifically measured per-user activation levels.
- Data must be fully local at runtime; no API fetch needed for content.

### 4.3 Initial 12 muscle groups

`pectorals`, `deltoids`, `trapezius`, `latissimus-dorsi`, `biceps`, `triceps`, `rectus-abdominis`, `obliques`, `gluteals`, `quadriceps`, `hamstrings`, `calves`.

For each group, create both a catalog entry and an explicit mesh coverage audit; any model that lacks required meshes blocks completion of that group. Bilateral/midline grouping rules must be documented.

### 4.4 Initial 10 exercises

| ID | Name | Initial primary groups | Supporting examples; review before ship |
| --- | --- | --- | --- |
| `bodyweight-squat` | Bodyweight Squat | quadriceps, gluteals | calves/core stabilization |
| `leg-press` | Leg Press | quadriceps, gluteals | calves; varies by foot placement |
| `chest-press` | Chest Press | pectorals | triceps, deltoids |
| `push-up` | Push-up | pectorals | triceps, deltoids, core stabilizers |
| `lat-pulldown` | Lat Pulldown | latissimus-dorsi | biceps, upper back |
| `seated-row` | Seated Cable Row | latissimus-dorsi, trapezius | biceps; technique-dependent |
| `shoulder-press` | Shoulder Press | deltoids | triceps, trapezius |
| `biceps-curl` | Biceps Curl | biceps | no mandatory secondary group |
| `triceps-pushdown` | Triceps Pushdown | triceps | no mandatory secondary group |
| `romanian-deadlift` | Romanian Deadlift | hamstrings, gluteals | back/core stabilizers |

**Content gate:** Review descriptions and role mappings against reputable anatomy/exercise references before publishing. Do not imply equal recruitment for all variants or falsely visualize muscle contractions.

### 4.5 Example JSON record

```json
{
  "id": "latissimus-dorsi",
  "label": "Latissimus Dorsi",
  "anatomicalNames": ["Latissimus dorsi"],
  "region": "back",
  "side": "bilateral",
  "description": "A large back muscle involved in pulling the upper arm down and backward.",
  "actions": ["Shoulder adduction", "Shoulder extension"],
  "meshIds": ["latissimus_dorsi_L", "latissimus_dorsi_R"],
  "aliases": ["lats", "back"],
  "defaultCamera": "back"
}
```

Mesh IDs above are a **proposed naming convention**; replace with validated IDs emitted by the real model export, not guesses.

## 5. Anatomical asset preparation pipeline

### 5.1 Licensing gate (mandatory before shipping)

Candidate source: [Z-Anatomy](https://github.com/Z-Anatomy/Models-of-human-anatomy). Its repository describes CC BY-SA 4.0 licensing and includes BodyParts3D derivations and references to other assets with their own terms. **Do not redistribute the entire source model blindly.** Audit the selected muscle meshes and source provenance, exclude third-party material with conflicting licenses, and document final licenses and attribution. If derivatives must be share-alike, publish the derived model files under the applicable license, while seeking legal review if redistribution boundaries are unclear.

Required `docs/ASSET_PROVENANCE.md`: source URL, downloaded version/commit, license per component, source attribution text, transformations, excluded mesh sources, derivative license and download link when required, and reviewer sign-off.

### 5.2 Blender authoring requirements

1. Acquire verified source model on development machine; never assume source archive structure or file names.
2. Identify the 12 required **surface** groups and underlying left/right/midline muscle meshes; remove unrelated organs, nerves, bones, labels, and optional hidden layers from MVP export.
3. Use consistent transform: upright +Y, anatomical anterior toward +Z, model centered around torso/origin; record coordinate mapping in a machine-readable export report.
4. Apply transforms and correct outward-facing normals; remove duplicate geometry and extraneous animation tracks/material slots.
5. Preserve distinct named objects/material IDs for tap/selection; parent under appropriate logical groups. Do not merge all muscles into one anonymous selectable mesh.
6. Build a reviewed mapping from source object name to stable app mesh ID, with tests for missing/duplicate IDs and mirrored anatomy.
7. Reduce polygons conservatively; keep recognizable major boundaries and tendons. Optimize mesh/material count and consider Meshopt geometry compression; compare with Draco on the M1 iPad rather than assuming one is faster.
8. Use texture atlases/KTX2 only if they improve measured memory/load behavior. Bundle any decompressor WASM/JS locally.
9. Export `muscles-core.glb` and generate `mesh-report.json`: mesh IDs, triangle counts, bounding boxes, size, source mapping.
10. Produce `pack-manifest.json` with path, byte length and SHA-256 for each file. Validate loading, required IDs, bounding boxes, and camera framing automatically.
11. Capture front/back/side reference screenshots for visual review and documentation.

### 5.3 Target asset-budget acceptance

- Preferred pack ≤40 MB downloaded; revise with measured reasons if anatomical quality requires more.
- No external texture URLs. Decoder dependencies available offline.
- 12 groups visually distinct and tappable at standard iPad landscape zoom.
- No broken normals, black surfaces, invisible groups, floating separated meshes, or left/right inversion.
- Device profiling done at typical zoom and a full 360° orbit; measurable loading time and frame rate recorded.

### 5.4 Temporary fallback

Until licensed anatomy meshes are reviewed, agent may ship a **clearly labeled developer-only placeholder** (procedural body silhouette/primitive meshes). It must not be represented as anatomically accurate or used to claim the final acceptance gates have passed.

## 6. Offline pack design — exact behavior

### 6.1 Storage architecture

1. Workbox precaches hashed app-shell JS/CSS/HTML, local icon/fonts, small placeholder, and any required decoder bundles. Shell's navigation route resolves offline.
2. Large GLB belongs to a **user-initiated** versioned core pack, stored in Cache Storage under `musclevision-pack-core-<version>`; avoid cache-first fetching that could leave unverified half-packs marked ready.
3. IndexedDB records `activePackVersion`, `packStatus`, `lastVerifiedAt`, compatible app version and preferences. Metadata is an optimization; actual cache resource presence and checksum determine final readiness.
4. Core pack manifest is provided with the app release and/or downloaded into staged cache. It has version, exact expected files, lengths and checksums.
5. All pack URLs are same-origin and versioned, e.g. `/packs/core-v1/muscles-core.glb`; ensure the Service Worker can serve these resources from verified versioned caches when offline.

### 6.2 Installation state machine

`NOT_INSTALLED → DOWNLOADING → VALIDATING → READY`

Possible deviations:
- `DOWNLOADING → ERROR_NETWORK / ERROR_QUOTA / CANCELED → RETRY`
- `VALIDATING → ERROR_CHECKSUM / ERROR_INVALID_MODEL → RETRY`
- `READY → UPDATE_AVAILABLE → DOWNLOADING_NEW → VALIDATING_NEW → READY_NEW`
- `READY → REPAIR_REQUIRED` only if the active pack fails integrity verification.

Algorithm:
1. On explicit download, check connectivity by attempting the real manifest/file request, report storage `estimate()` if supported and request `persist()` via the user interaction when available.
2. Stage files to `musclevision-pack-core-staging-<version>` sequentially or with max concurrency 2.
3. Stream/download progress in bytes. Validate each response status/type and actual byte length; SHA-256 hash the resulting content (Web Crypto). Cache only validated responses. Release large buffers promptly.
4. Once all entries are present, cross-check manifest, cache reads and GLB parser/mesh registry; treat unresolved mesh IDs as invalid pack.
5. Promote the staged data to a **ready pack cache**, or keep the complete staged cache as the ready cache with its immutable versioned name. **Only then** atomically set the active pack pointer in a single IndexedDB transaction. A pointer is never set before complete validation.
6. Keep prior active cache during an upgrade. Delete old packs only after new pack is verified, activated and not in use by an existing tab/session; if unsure, defer cleanup until next safe startup.
7. If interrupted, keep the old version active; on restart inspect and resume or restart staging without advertising readiness.
8. Cache missing/corrupt detection triggers explicit repair UI. Do not silently fall back to online fetching while displaying `Offline Ready`.

**Compatibility:** App and asset pack use manifest `minAppVersion` and schema checks. Do not promote incompatible pack; don't delete currently usable shell/pack during an unverified update. Service Worker update should be user-confirmed when a new app release risks compatibility, not unconditional `skipWaiting`.

**Ready means:** Service Worker installed/controlling app, shell navigation cached, active pack manifest compatible, all mandatory file hashes valid, and GLB registry validated at least once. On normal launches, perform a cheap existence/version check immediately and a periodic/full hash check on demand or when corruption suspected; avoid hashing tens of MB on every navigation. The UI must distinguish last-verified readiness from a currently running verification.

### 6.3 Offline UX guarantees

- `Ready`: viewer launches and searches/exercise mappings work in Airplane Mode.
- `Shell-only`: user can see branding/help but cannot see the 3D model; provide accurate install guidance.
- `Partial pack`: never ready, clear progress/error, clean retry.
- `Corrupt/evicted`: never show a blank canvas; show `Repair anatomy pack` with explanation. Repair needs internet.
- Update cannot render already-installed version unusable because a new download fails.
- Avoid dependence on `navigator.onLine` alone; actual request/verified cache controls the result.
- Home Screen and Safari contexts may have separate website-data handling; test in the installed app specifically.

## 7. Rendering and input implementation

- Use named mesh registry from GLB scene traversal; registry built at load, not from matching strings at every click.
- Raycast with appropriate mesh instances and touch pointer-events; map clicked source mesh to stable muscle group.
- Preserve original materials; implement select/highlight/dim roles with cloned materials or controlled uniforms, and dispose of generated resources correctly.
- Lazy-load GLB once per active pack and reuse in one scene. Never reinstantiate scene on selected muscle/exercise changes.
- Default model camera framed from verified bounding box, not hardcoded world units only. Support `Front/Back/Left/Right` relative to documented +Z anterior orientation.
- Clamp device-pixel ratio to a measured range (initial cap 1.5–2); scale lighting/shadows down if needed. Disable overly costly postprocessing on iPad.
- Pause/downgrade rendering when scene is not visible; use on-demand rendering when practical. Respect reduced motion.
- WebGL context loss: show nontechnical recovery status, attempt controlled renderer recreation without losing UI state; prevent infinite retry loops.
- Touch behavior: avoid accidental orbit when tapping; click only under small pointer-travel threshold; ensure canvas touch-action behavior allows controlled gestures without breaking page navigation.

## 8. Privacy, accessibility, and educational safety

- No account, camera, microphone, motion permissions, medical input, or analytics needed in v1.
- Preferences stored locally only. No personally identifiable user data transmitted by the app.
- All key tasks possible through labeled controls/search in addition to direct mesh tapping; panel has usable focus order and accessible contrast.
- Legible at iPad arm's length, ≥44 CSS px tap targets, no information solely encoded by highlight color.
- Educational label: `Muscle involvement varies with technique and individual anatomy. This is an educational visualization, not an EMG measurement, medical diagnosis, or rehabilitation recommendation.`
- Attribution/legal notices available without network and documented in repo.

## 9. Test plan and acceptance matrix

**Automation:** Vitest for data/schemas/state machines/cache logic; React Testing Library for panels; Playwright for desktop smoke/keyboard/interactions, offline flows and viewport transitions. Automated desktop WebKit coverage helps, but **does not replace manual testing in iPadOS Safari Home Screen mode**.

| Test ID | Setup / steps | Expected |
| --- | --- | --- |
| OF-01 | Fresh device, open online, install pack | Progress + validation complete; ready only when complete |
| OF-02 | Install Home Screen PWA, enable Airplane Mode, force quit, reopen | Viewer boots with actual 3D model; search/exercises fully work |
| OF-03 | With pack ready, reboot iPad offline, reopen | Same offline success without HTTP fetches |
| OF-04 | Throttle/intercept pack request and cut network at 50% | Not-ready state; accurate error; retry recovers |
| OF-05 | Tamper file/checksum in test environment | Validation fails; no ready state |
| OF-06 | Simulate quota exceeded on Cache API write | Clear storage error; old installed version untouched |
| OF-07 | Download v2 while v1 ready, fail validation | v1 remains active/usable offline |
| OF-08 | Kill app during v2 staging | On relaunch, v1 remains ready; staging recovers or resets safely |
| OF-09 | Delete/evict a mandatory cached file, reopen | Detect failure; show repair-required, never blank canvas |
| OF-10 | Switch offline/online with active model loaded | No missing dependencies or forced re-download |
| OF-11 | Open in standalone Home Screen vs Safari tab | Both clearly show their **own verified** status, not assumed shared storage |
| OF-12 | Offline open `/exercises` after installation | SPA navigation resolves offline and displays content |
| XR-01 | Tap every one of the 12 groups | Correct panel ID and corresponding meshes highlighted |
| XR-02 | Select each of 10 exercises | Correct primary/secondary/stabilizer mapping and legend |
| XR-03 | Rotate/pinch/reset; orientation change | No stuck camera, lost selection or model reload |
| XR-04 | Rapidly switch exercise/muscle modes 30× | No stale selection, visual bleed or runaway memory growth |
| XR-05 | Simulate GLB load error/WebGL context loss | Recoverable error UI, no permanent blank screen |
| QA-01 | Perform 20-minute 3D demo on M1 iPad | No crash, record FPS/memory/thermal behavior |
| QA-02 | Accessibility audit + reduced motion | Touch targets, contrast, labels, motion settings acceptable |
| LIC-01 | Review packed model source licenses/provenance | Attribution + redistribution conditions satisfied |

**Release device matrix:** iPad M1 Safari standalone (required), iPad M1 Safari tab (required), phone Safari compact view (smoke), macOS Safari/Chrome desktop (automated). Document specific iPadOS version, physical model, available disk and network test procedure in `docs/IPAD_TEST_MATRIX.md`.

## 10. Phased GitHub issue backlog

Each issue should be a focused PR with implementation, tests, screenshots/screen recording where visual, and a checklist linking to relevant acceptance IDs. No giant all-in-one PR.

### Phase 0 — Repository / feasibility (P0, blockers)

**MV-001 — Bootstrap PWA and CI**  
Implement React/Vite/TypeScript, lint/format/typecheck/unit test commands, GitHub Actions, responsive shell and manifest.  
**Done when:** `pnpm install && pnpm lint && pnpm typecheck && pnpm test && pnpm build` pass; HTTPS preview can be added to Home Screen.

**MV-002 — Licensing and model feasibility spike**  
Assess candidate anatomical sources, create `ASSET_PROVENANCE.md`, import/inspect model in Blender, identify source meshes for all 12 groups.  
**Done when:** license audit documented; 12 required groups are traceable to source meshes OR explicit blocker recorded. **No final asset PR without this.**

**MV-003 — iPad 3D performance proof**  
Load a representative draft GLB on real M1 iPad; benchmark interactive orbit, memory and loading; confirm locally packaged decoders.  
**Done when:** raw measurements documented and choice of compression + fidelity approved.

### Phase 1 — Core app and anatomy content

**MV-004 — Typed domain catalogs and validators**  
Implement `MuscleGroup`, `Exercise`, `AssetPackManifest`, JSON for 12/10 records, ID/relation validation.  
**Done when:** malformed or missing references fail tests; content review checklist present.

**MV-005 — Export and validate GLB asset pack**  
Author Blender pipeline, named mesh report, glTF validation, compression and SHA manifest.  
**Depends on:** MV-002. **Done when:** all 12 groups map to renderable mesh IDs; no incompatible license issues; CI mesh audit passes.

**MV-006 — 3D viewer and camera controls**  
Create lazy-loaded scene, orbit/pinch, front/back/side/reset, visibility and loading/error UI.  
**Depends on:** MV-003/MV-005. **Done when:** XR-03, XR-05, QA-01 preliminary pass.

### Phase 2 — Teaching interaction

**MV-007 — Mesh picking and highlight/isolate**  
Implement raycast + mesh registry + material role states + tap-versus-drag behavior.  
**Done when:** XR-01 passes; all muscle pick tests are deterministic.

**MV-008 — Muscle details and offline search**  
Create detail panel, local index, keyboard/touch flow, aliases and related exercises.  
**Done when:** every group is searchable and details match highlighted meshes offline.

**MV-009 — Exercise Explorer and role legend**  
Create 10-exercise browser, role-aware group highlighting and cross-navigation.  
**Done when:** XR-02/XR-04 pass; no activation percentages are presented.

### Phase 3 — Offline install and reliability

**MV-010 — Service Worker / app-shell precache**  
Use `injectManifest`; cache shell, offline navigation, local decoder/fallback dependencies.  
**Done when:** shell can start in Airplane Mode, even before anatomy pack installed.

**MV-011 — Atomic pack installer / verifier**  
Implement Cache Storage staging, checksums, progress, IndexedDB active pointer, quota/network failure handling.  
**Depends on:** MV-005/MV-010. **Done when:** OF-01/04/05/06 pass.

**MV-012 — Offline settings, repair and version upgrades**  
Add truthful readiness, Verify, Repair, update rollback, storage info and Home Screen instructions.  
**Done when:** OF-02/03/07/08/09/10/11/12 pass on real iPad where applicable.

### Phase 4 — Polish, device QA and release

**MV-013 — iPad UX, accessibility and rendering optimizations**  
Refine layout, bottom sheet, high-DPI/raster performance, low-memory lifecycle, touch targets, reduced motion.  
**Done when:** QA-01/02 and 20-minute gym demo pass.

**MV-014 — Attribution, content review and legal release gate**  
Confirm mappings, licenses, share-alike publication obligations, attribution and disclaimers.  
**Done when:** LIC-01 passes and content reviewer signs off.

**MV-015 — Release pipeline, runbook and final offline certification**  
Document deployment, update rollout/rollback, app version and pack manifest, device install instructions, offline verification evidence and screenshots.  
**Done when:** every MVP acceptance ID passes, known limitations recorded, v1.0 tag released.

### Recommended PR order

1. `MV-001` and `MV-002` independently.
2. `MV-003` before investing in polished UI or full-resolution exports.
3. `MV-004` + `MV-005` before expecting accurate selection.
4. `MV-006` → `MV-007` → `MV-008` → `MV-009`.
5. `MV-010` in parallel with UI development, then `MV-011` → `MV-012`.
6. `MV-013` → `MV-014` → `MV-015` as release gates.

## 11. Coding-agent instructions

- Read this PRD plus `ASSET_PROVENANCE.md` and `IPAD_TEST_MATRIX.md` before making changes.
- Work on **one `MV-###` issue per PR**; state scope, files changed, tests run, screenshots, device verification, blockers and follow-ups.
- Never silently replace actual anatomical meshes with placeholders and call the issue complete; do not fetch unlicensed or unreviewed third-party assets into the shipped bundle.
- No backend, OAuth, Supabase/Firebase, analytics, runtime CDN, paid SDK or camera permissions without a separately approved scope change.
- Keep domain model IDs and view/rendering code decoupled. Add tests for every data mapping or offline state transition.
- A desktop emulator is not proof of iPad offline operation; mark physical-device checks as pending if not personally performed.
- Each PR must pass `pnpm lint`, `pnpm typecheck`, `pnpm test`, `pnpm build`; run Playwright subset for relevant features.
- Prefer incremental, reversible changes; when an asset or license gate is blocked, submit documented findings rather than inventing completion.

### Template: GitHub issue / coding agent task

```md
## Objective
Implement MV-###: <short name>

## Inputs
- docs/PRD.md sections: <sections>
- Depends on: <issue IDs>

## Scope
- <specific deliverables>

## Non-goals
- <explicit exclusions>

## Acceptance criteria
- [ ] <observable behavior>
- [ ] <unit/E2E tests>
- [ ] <iPad-specific check, if applicable>
- [ ] <offline/license check, if applicable>

## Definition of done
- [ ] lint/typecheck/tests/build pass
- [ ] screenshots or demo evidence provided
- [ ] no new external runtime dependencies
- [ ] blockers and follow-ups explicitly documented
```

## 12. Release definition of done

Release v1.0 only when:

- [ ] All 12 groups display and tap-select their intended meshes.
- [ ] All 10 exercises correctly color mapped muscle groups.
- [ ] Front/back/side/reset and touch navigation work on actual M1 iPad.
- [ ] Standalone app works fully in Airplane Mode after relaunch **and reboot**.
- [ ] Failed downloads/updates never masquerade as ready or damage old installed pack.
- [ ] Missing/evicted pack prompts repair instead of blank screen.
- [ ] There are no runtime third-party network dependencies.
- [ ] Accessibility and 20-minute device session pass.
- [ ] Anatomical content and asset provenance/derivative licensing signed off.
- [ ] PRD, runbook, test evidence and release tag exist.

## 13. Verified technical references (accessed 2026-10-18)

1. WebKit, *Updates to Storage Policy*: https://webkit.org/blog/14403/updates-to-storage-policy/
2. WebKit, *Tracking Prevention in WebKit* (Home Screen web-app first-party storage exception): https://webkit.org/tracking-prevention/
3. Three.js, `GLTFLoader` (Meshopt, Draco, KTX2 hooks): https://threejs.org/docs/pages/GLTFLoader
4. Vite PWA, *Advanced injectManifest Guide*: https://github.com/vite-pwa/vite-plugin-pwa/blob/main/docs/guide/inject-manifest.md
5. Z-Anatomy repository and licensing: https://github.com/Z-Anatomy/Models-of-human-anatomy and https://github.com/Z-Anatomy/Models-of-human-anatomy/blob/master/License.txt

**Open release dependency:** Selection, cleanup and licensing verification of the actual anatomical meshes. The architecture and screens can be implemented without pretending that the final model has already been sourced or benchmarked.
