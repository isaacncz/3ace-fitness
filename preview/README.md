# 3ACE Fitness — interactive preview

[Open the private clickable preview](https://threeace-workout-preview.wanchinlim99.chatgpt.site)

A browser-local workout-logging prototype with synthetic sample records. It opens directly into an active workout. Try editing weight/reps, completing sets, adding an exercise, finishing, viewing history and repeating a routine. Settings offers unit conversion, export and reset.

Serve this directory over HTTP, for example `python -m http.server 4173 --directory preview` from the repository root. No installation or build is needed. The hosted distribution also offers a source.zip generated from these text sources. Downloading it locally is optional.

## Reuse

The unmodified `vendor/workout-model.js` comes from DuarteSantos8/openGym commit b6cfe2bbd3f099523ffbf2677438864e9a09a06f. It provides set-phase and completed-volume semantics. The interface and local preview session flow are new work informed by openGym's prefilled sets, exercise menus and rest timer. This is a small prototype using selected upstream logic, not a port of openGym's full backend or sync architecture.

The preview is distributed under GNU AGPL-3.0. See LICENSE and NOTICE.txt. No restricted exercise media is included.

## Validation

Chromium checks at 390x844 and 1440x1100 passed: set completion, keyboard focus after completion, numeric edits, reload recovery, exercise search/add, finishing with only completed sets, and history values. Initial screenshots were visually reviewed; a separate reviewer found a focus-restoration issue, then verified its fix. No physical iPhone/Safari, offline-installation, production data reliability or backend behavior is claimed. Browser-local storage is prototype persistence and can be cleared.

## Photo-first coaching flow

The latest preview starts with a client selector and a camera/library photo action. Attach a session reference, choose an exercise, enter weight/reps and Log set. First set starts the session. Repeat sets keep the last values. Client portraits are optional, and switching clients preserves separate ongoing sessions. End session saves directly, with Reopen available. Session photos appear in client-filtered History.

Photos are stored locally in IndexedDB; no photo is sent to a model or backend. The JSON export contains client/workout metadata, not the photo blobs. Client identity, exercise recognition, workout-plan OCR and rep counting are not inferred. Camera launch depends on the device/browser; file-input behavior was checked in Chromium, physical phone capture remains unverified.

The browser checks cover camera-input attributes, photo attachment, image reload recovery, client portraits, per-client default/history isolation, repeated set logging, direct finish/reopen and photos in History at both viewport sizes. Test image fixtures contain no real client data.
