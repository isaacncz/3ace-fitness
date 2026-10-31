# 3ACE Fitness — Product brief

Status: workout-logger direction with a clickable prototype for review, 2026-10-31.

## Confirmed by the user

- Build around workout logging, simpler than the referenced Open Gym experience.
- Deliver as a web app.
- Apply Apple-inspired design philosophy: clarity, restraint, familiar behavior and thoughtful defaults.
- Keep the previous repository Markdown in a backlog folder.
- Review the logging experience through an interactive web mockup.

The user subsequently requested reuse of openGym's backbone and a preview. The reference is DuarteSantos8/openGym. preview/ reuses its workout-model module under AGPL-3.0 and implements a smaller browser-local logging flow; it does not port the full app.

## Recommended first release

Audience: a coach logging strength workouts for individual clients, with photos kept beside their sets.

Job: select a client, attach a workout photo, log only the exercise/weight/reps, and keep each client’s records separate. The first set starts a session; ending it saves directly with a reopen option.

Primary device: an iPhone-sized browser or installed Home Screen web app. Tablet and desktop remain usable.

Visitor mode: Operate. The core artifact is the active workout, with previous performance beside today's editable set values.

Scope: strength exercises, custom exercises, reusable routines, one active workout, weight/reps sets, warm-up labels, rest timer, history, exercise progress and JSON export/restore. Strength-only and these feature details remain recommendations, not confirmed choices.

Success targets to verify during implementation:
- A familiar prefilled set can usually be completed with one tap.
- A routine can be started within two deliberate actions from Train.
- A successfully saved set survives refresh and relaunch.
- Logging and history work offline after a verified initial installation.
- Finishing a workout does not discard completed sets or silently include unfinished ones.

## Suggested boundaries

Local-first, without accounts or cloud sync. Browser data can be cleared or evicted; portable backups must be available. Recommend React, TypeScript, Vite, IndexedDB and a service worker; exact dependencies remain implementation choices.

The photo-first prototype now supports camera/library image inputs, optional client portraits, client-specific sessions/history and browser-local photo storage. Photos are references; no OCR, face recognition, exercise recognition or automatic rep counting is implemented. Defer cardio-specific logging, supersets, training-plan generation, social feeds, body measurements, calorie estimates, AI coaching, camera analysis, Apple Watch integration and 3D anatomy.

## Open decisions

- Confirm strength-only MVP versus cardio.
- Confirm whether multi-device sync is a later requirement.
- Review the proposed visual direction and workout flow.

These decisions do not prevent review of the documented design.

