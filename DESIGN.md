# 3ACE Fitness — Proposed design direction

Status: recommendation for review. No implemented UI, approved visual mockup or measured device evidence yet.

## Thesis

A calm training notebook. Previous performance sits beside today's input, and completing a set is the dominant action. Apple-inspired principles guide hierarchy and behavior; this is an independent web interface.

Mode: Operate. First surface: Train and its active-workout state.

## Visual language

- Use the platform system font stack, with tabular numerals for weights, reps and time. No font downloads.
- Light theme: canvas #F2F2F7, surfaces #FFFFFF, text #1C1C1E, secondary text #636366, action blue #005AC1.
- Dark theme: canvas #000000, surfaces #1C1C1E, text #F5F5F7, secondary text #AEAEB2, action blue #64A8FF.
- Completion uses a check symbol, accessible text and a subdued filled treatment; color alone never carries state.
- Hairline separators organize rows. Use surfaces for meaningful groups rather than boxing every value.
- Spacing scale: 4, 8, 12, 16, 24, 32. Typical phone horizontal inset 20 CSS px.
- Screen title 32/38 semibold; exercise title 20/26 semibold; body and editable numbers 17/24; supporting labels 13/18. Support text enlargement and reflow.
- Group corner radius 16px; input radius 10px; buttons 12px. Reserve pills for small status labels.
- No gradients, decorative charts, exercise stock photography or glass effects. Motion explains a state change, typically 120–200ms; respect reduced motion.
- Default theme follows the device, with an optional setting. All final token pairs require contrast checks.

## Layout

Mobile: three labeled bottom tabs, Train / History / Routines. Account for safe-area insets. Settings is a toolbar action. Active sessions are full-page; navigation remains possible without ending them.

The resting Train screen has a large title, one primary action and compact routine/recent-workout rows. An active session replaces this with the logging workspace immediately. No statistics dashboard precedes the task.

The workout screen uses an editable title, a quiet elapsed time and a Finish action. Exercises stack vertically. Each has a Previous / Weight / Reps / Done set table, followed by Add set. The current editable row has the strongest local emphasis; completed rows remain legible and editable.

At compact widths, shorten column labels and place units in the header. Preserve 44px minimum controls and reflow supporting content instead of shrinking targets. Treat keyboard overlap and enlarged text as acceptance cases.

Tablet/desktop: restrained centered content. Wider layouts can show the exercise outline beside the active exercise; do not stretch numeric fields across the viewport or create a second workflow. Use a practical content width around 960px.

## Signature interaction

Complete a prefilled set with one tap. Persist the completion first, then show its check, start the configured timer and quietly emphasize the next row without opening the keyboard or scrolling unexpectedly. Pending and failed writes must never look saved.

When values need changing, tapping a value selects it for numeric entry. Do not rely on horizontal dragging, swipes or long presses for essential functions.

## Overlays and feedback

Exercise picker: searchable sheet with recent exercises first and a visible Create exercise option. Sheets have explicit close/back controls, accessible focus and keyboard dismissal.

Rest timer: compact persistent strip above bottom navigation, with remaining time, +30s and Skip. It survives navigation. Background completion is reconciled from timestamps on resume; foreground sound is optional, and background notifications are not promised.

Save errors: readable inline message with Retry. Finish never acts as a workaround for a failed save. Destructive actions have explicit names; discard-session confirmation explains that completed sets will be removed.

## Content and accessibility

Use direct verbs: Start workout, Add exercise, Add set, Finish workout, Save routine, Export backup. Avoid motivational filler and unsupported performance claims.

Semantic controls, visible focus, screen-reader labels containing exercise and set context, 44px minimum targets, contrast checks, reduced motion and enlarged-text support are requirements. Use decimal weight and integer reps inputs with visible labels. Charts include a readable data list.

## Review references

Apple-inspired clarity and familiar mobile conventions are user direction. No exact competitor layout or Apple visual asset is adopted. The proposal wireframes use synthetic example workout data.
