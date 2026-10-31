# 3ACE Fitness — Product brief

Status: proposed workout-logger direction, 2026-10-31.

## Confirmed by the user

- Build around workout logging, simpler than the referenced Open Gym experience.
- Deliver as a web app.
- Apply Apple-inspired design philosophy: clarity, restraint, familiar behavior and thoughtful defaults.
- Keep the previous repository Markdown in a backlog folder.
- Propose the design before application implementation.

The exact Open Gym product has not been identified; no competitor-specific behavior is assumed.

## Recommended first release

Audience: an individual who knows their exercises and wants to record strength workouts between sets.

Job: begin a session, record accurate sets with minimal interaction, and find previous performance next time.

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

Defer cardio-specific logging, supersets, training-plan generation, social feeds, body measurements, calorie estimates, AI coaching, camera analysis, Apple Watch integration and 3D anatomy.

## Open decisions

- Confirm strength-only MVP versus cardio.
- Confirm whether multi-device sync is a later requirement.
- Review the proposed visual direction and workout flow.

These decisions do not prevent review of the documented design.
