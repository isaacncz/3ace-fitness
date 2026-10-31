# 3ACE Fitness — Agent instructions

## Current task and authority

The user has selected a simpler workout logger delivered as a web app, with Apple-inspired design principles. The original MuscleVision Markdown has been archived at the user's request.

Read PRODUCT.md, DESIGN.md and docs/WORKOUT_LOGGER_PROPOSAL.md before changing the current product. They explicitly distinguish confirmed decisions from proposed details.

## Archive

backlog/musclevision/ preserves the original files byte-for-byte and in their original relative structure. Its requirements describe that deferred anatomy project; they do not define the workout logger. Treat the archive as historical reference, not active implementation instructions.

## Working rules

- Preserve the archive unless explicitly asked to revise it.
- Keep documentation and implementation status truthful; proposals are not shipped features.
- Keep changes focused and reviewable.
- Do not add an account, backend, analytics, AI, camera or 3D dependency as an incidental implementation choice.
- Before implementing, resolve material open product choices rather than silently treating recommendations as confirmed.
- For the proposed logger, prioritize durable set writes, session recovery, accessible inputs and export/restore.
- Once application tooling exists, run checks appropriate to the changed behavior. Do not claim device, browser or offline checks without evidence.
