# 3ACE Fitness — Workout logger proposal

Status: design proposal for review, 2026-10-31. No application implementation is included.

## 1. Product promise

Open the app, know what you did last time, and log the next set in seconds.

Recommended first release: strength training, with accurate durable logging as the core. The user has confirmed the web-app platform and restrained Apple-inspired direction; strength-only scope and the details below are recommendations.

## 2. Navigation and screens

| Surface | Primary job | Main action |
| --- | --- | --- |
| Train, idle | Begin or resume training | Start workout / Resume workout |
| Active workout | Record sets | Complete set |
| History | Recall previous sessions | Open a workout |
| Workout detail | Inspect or correct a saved session | Edit workout |
| Exercise detail | See previous performance | View session |
| Routines | Prepare reusable exercise lists | Start routine / New routine |
| Settings | Units, timer, appearance and backups | Export backup |

Settings stays outside primary tab navigation. Exercise details are reached through exercise names, not an additional tab.

### Train, first visit

Show the title Train, a short explanation and Start workout. Offer Create a routine as a secondary action. No registration or onboarding carousel. Ask for kg/lb on first numeric entry, using the device locale only as an editable default suggestion.

### Train, returning user

A routine list and recent session support quick starts. If there is an active workout, Resume workout is primary; starting a second workout is prevented until the existing one is finished or explicitly discarded.

### Active workout

Synthetic example, structure rather than pixel-accurate mockup:

```text
‹ Train                  Finish
Push A
24:18 elapsed

Bench Press                 ···
Set   Previous   kg    Reps
1     40 × 10    40     10   ✓
2     40 × 10    40     10   ○
3     40 ×  8    40      8   ○
+ Add set

Incline Dumbbell Press      ···
Set   Previous   kg    Reps
1     16 × 10    16     10   ○
2     16 × 10    16     10   ○
+ Add set

+ Add exercise

Rest  01:24       +30s   Skip
Train       History    Routines
```

The resting screen and active workout occupy the same Train destination. Exercise menus contain reorder, notes, rest duration and remove. Routine order remains stable unless explicitly edited.

### History and progress

History is a chronological list grouped by month. Each row shows workout name, date, duration and completed working-set count. Workout details show exactly what was logged and allow explicit edits.

Exercise detail offers Last session and a restrained best-working-set-weight trend, labeled with reps. Bodyweight-only exercises use reps. Do not combine kg and lb values without conversion or call an estimated number a measured strength score. Hide charts until enough data exists and show the underlying records.

### Routines

Each routine stores ordered exercise IDs, planned set counts, optional target reps and rest settings. Keep editing to a list of exercises and inline set targets. Starting a routine copies its plan into a new session. Subsequent session edits do not silently mutate the routine.

After Finish, an optional Save as routine action can capture an ad hoc workout.

## 3. Logging rules

- One active workout per local dataset.
- Each exercise and set has a stable ID. Historical sessions retain names and equipment snapshots when a custom exercise is renamed or removed.
- Seed values from the latest completed session for the same exercise and set type; match by set order. Fall back to routine targets, then an empty weight/reps row. A populated value is still uncompleted until explicitly checked.
- Previous always refers to the prior completed workout, not the row immediately above it. Show a dash when no comparable set exists.
- Add set copies the most recent valid values for that exercise as uncompleted. Warm-ups are visibly marked and excluded from working-set records.
- Weight may be zero for bodyweight/no-external-load exercises. Reps must be a positive integer. Reject negative, nonfinite and blank required values with inline feedback.
- A weight convention is shown per exercise: total barbell load, per-hand dumbbell load, external bodyweight load or machine stack reading. Avoid silently comparing unlike conventions.
- Store load canonically with decimal-safe precision and explicit display units. Switching kg/lb changes presentation, not historical values through repeated rounding.
- Set completion is committed atomically before saved feedback or timer side effects. Rapid repeated taps must not duplicate a set or timer.
- Unchecking a set preserves its values and does not start another timer.
- Completed rows remain editable; corrections save explicitly and do not restart rest.
- Finish shows completed-set count and identifies unfinished rows. Only completed sets enter history; the user can return to logging before confirming.
- No completed sets: offer Continue or Discard, rather than creating an empty workout.
- Elapsed time uses session timestamps and represents wall-clock session duration, including time away. A recovered long session can have its end time corrected before Finish.

## 4. Rest timer

Default recommendation: 90 seconds, editable per exercise. Completing a set replaces the current rest countdown with the newly configured duration. +30s extends the target end timestamp; Skip clears it.

Persist the target end timestamp, not an incrementing counter. Reconcile on visibility change and relaunch. If time expired in the background, display Rest complete on return. A browser app cannot promise an audible alert while suspended; do not present that as a supported guarantee.

## 5. Reliability and web behavior

Recommend React + TypeScript + Vite, IndexedDB for structured workout records and a service worker for the application shell. Keep the domain model independent of UI components.

- Successful set writes, active session and timer state survive refresh/relaunch.
- Make persisted state authoritative and recover interrupted drafts without losing completed sets.
- Prevent conflicting active-session writes across tabs through a single-writer mechanism or transactional revision checks; a second tab can offer read-only viewing.
- Quota/write failure: retain the visible draft, show failure and Retry, and offer export of recoverable data. Never show a success check on failure.
- Service-worker updates wait until an active workout is finished or the user chooses a safe reload. No forced update mid-set.
- Offline operation requires a successfully cached shell. Verify app routes and local data access before reporting offline availability.
- Request persistent storage when supported without promising permanence. Settings explains that clearing site data removes local records.
- Versioned JSON export includes workouts, routines, custom exercises and settings. Restore validates schema, version, references, file size and record counts before writing in a transaction.
- Restore preview shows counts; same-ID/same-content records deduplicate. Same-ID/different-content conflicts must be resolved explicitly before import. Repeating an import must not duplicate records.
- Review limits for backup size and record counts before implementation; do not invent unlimited storage.
- First release has no cross-device sync. Export/restore is deliberate transfer, not continuous synchronization.

## 6. Key states

| State | Expected behavior |
| --- | --- |
| No history | Explain that completed workouts appear here; offer Start workout |
| No routines | Offer New routine and an ad hoc workout |
| No exercise match | Create exercise with the search text prefilled |
| Relaunch during workout | Restore saved sets and offer Resume |
| Invalid numeric input | Inline explanation; preserve entered text for correction |
| Write failure | Explicit unsaved state, Retry, no false completion |
| Offline after installation | Logging, routines and history continue locally |
| First visit offline | Honest unavailable state; no claim that installation occurred |
| Restore failure | Existing records remain unchanged |
| Deleting exercise/routine | Preserve historical workout snapshots |
| Discarding active workout | Name the destructive outcome and require confirmation |
| Enlarged text / keyboard open | Inputs, errors and completion controls remain reachable |

## 7. Delivery sequence

1. Review this proposal and settle strength-only scope, sync expectations and the visual direction.
2. Build a single vertical slice: start a workout, add one exercise, edit and persist a set, reload, resume and finish.
3. Add last-session values, routine start/edit and history.
4. Add timestamp-based rest timer, units and warm-ups.
5. Add versioned backup/restore, schema migrations, multi-tab protection and offline shell/update behavior.
6. Refine phone, tablet and desktop layouts; validate installed iPhone Safari and browser behavior.

The first prototype should test the entire logging loop before expanding the exercise catalog.

## 8. Validation plan for future implementation

Automate behaviors where data loss or incorrect records matter: repeated-tap idempotency, transaction failure, reload recovery, finish semantics, kg/lb conversion, import conflicts/rollback and migrations. Browser tests cover logging with a numeric keyboard viewport, offline relaunch and app updates with an active session.

Manually verify on a physical iPhone: Safari and Home Screen mode, keyboard overlap, safe areas, background timer reconciliation, force-close recovery, storage errors where reproducible, and accessibility/text enlargement. Add tablet/desktop checks for the alternate layout. These checks are pending; no device behavior is claimed as tested.

Usability goals: a familiar prefilled set usually takes one tap; changing weight/reps is direct; previous values are readable during entry; no tutorial is needed to finish a workout.

## 9. Deferred work

Cardio-specific records, supersets, cloud accounts/sync, AI progression, camera rep counting, 3D anatomy, social feeds and wearable integration are outside the suggested first release. MuscleVision remains intact in the repository backlog.
