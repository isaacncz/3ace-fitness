# 3ACE Fitness

A mobile-first coaching workout logger for the web, with a photo-first interactive prototype.

**Status:** Clickable logging prototype available; production application work remains pending.

[Try the private preview](https://threeace-workout-preview.wanchinlim99.chatgpt.site) · [Prototype source and validation](preview/README.md)

The confirmed direction is a simpler workout logger with Apple-inspired clarity, implemented as a web app. The recommended first release focuses on strength training, fast set entry and reliable local storage.

- [Product brief](PRODUCT.md): confirmed direction, recommendations and scope.
- [Design direction](DESIGN.md): proposed visual system and interaction principles.
- [Workout logger proposal](docs/WORKOUT_LOGGER_PROPOSAL.md): screens, logging behavior, data rules and delivery sequence.
- [Archived MuscleVision plans](backlog/README.md): the previous anatomy project, preserved for future consideration.

The workout-logger details are proposals for review, not approved implementation requirements. The prototype reuses selected openGym workout-model logic under AGPL-3.0; see preview/NOTICE.txt. No account, cloud sync or AI service is proposed for the first release.


The latest preview supports selecting a client, capturing a client portrait or workout reference photo, and logging exercises/weight/reps beside it. Sessions start with the first logged set, stay separate per client, and end with one tap plus a Reopen option. Photos remain on this device; they are not interpreted by AI.
